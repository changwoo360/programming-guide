### 一. 环境配置
1. Python版本： 3.5+（添加了类型注解）
2. 安装模块依赖
```
pip3 install -r requirements.txt
```
3. 配置redis
```python
# settings.py
REDIS_CONF = {
    "HOST": "127.0.0.1",
    "PORT": 6379,
    "USER": "",
    "PASSWORD": "",
    "DB": 12,
    "TABLE": "proxypool"
}
```
4. 启动代理池
```
python3 run.py
```

### 二. 项目结构
```python
# proxypool

|—— core
    |—— __init__.py
    |—— scheduler.py   # 调度模块
    |—— spider.py      # 爬虫模块
    |—— filter.py      # 过滤模块
    |—— db.py          # 代理池模块
    |—— error.py       # 自定义错误
|—— spiders
    |—— __init__.py
    |—— qingting.py    # 蜻蜓代理爬虫文件
|—— __init__.py
|—— run.py             # 入口文件
|—— settings.py        # 配置文件
|—— utils.py           # 一些额外功能
|—— README.md          # readme文件
|—— requirements.txt   # 依赖模块

```

### 三. 代理池执行流程
![爬虫代理池流程图](http://ww1.sinaimg.cn/large/006tNc79ly1g5z7xqah3xj31ek0u0e1s.jpg)
1. 启动`run.py`,会调用`Scheduler.run`方法

2. 在`Scheduler.run`方法中，会分别开启两个进程。一个进程循环调用`Spider.run`方法(爬虫模块)，一个进程循环调用`ProxyFilter.run`方法(过滤模块)

3. 在爬虫模块中  
a. 首先通过反射获取在settings中注册的爬虫对象(蜻蜓代理)；  
b. 再调用爬虫对象的`run`方法，获取到代理列表；  
c. 将代理列表通过`format_data`方法格式化(dict->str)；  
d. 将格式化后的代理列表保存到代理池中  

4. 在过滤模块中  
a. 先通过`get_proxies`方法获取到执行数量的代理  
b. 基于`asyncio`模块注册一个事件循环`loop`  
c. 记录当下的时间戳，作为爬虫请求开始时间  
d. 通过`check_usable_proxies`方法传入代理和开始时间，返回一个异步任务列表  
e. 通过调用时间循环的`loop.run_until_complete`方法，传入异步任务列表  

5. 在代理池模块中  
   > 由于爬虫模块和过滤模块都需要调用redis数据库，因此将写出一个代理池模块`Redis`，将爬虫和过滤两个模块需要的功能封装，作为接口提供。  
  
   > `redis_obj = Redis()`。其目的有两个：Redis名称存在歧义，通过引用变量避免歧义；天然单例，在外部多次import redis_obj时，只会实例化一次`Redis()`
   
   a. `__init__()`，初始化redis对象和使用的表名  
   b. `get(num=1)`，获取代理。当不传参数或参数为1时，会调用`_get_one()`，返回一个响应时间短的代理；当传入指定参数时，会调用`_get_many(num)`，返回指定数量的代理列表。  
   c. `get_random(count)`，获取到随机执行数量的代理（与响应时间无关）。该接口提供给过滤模块使用。  
   d. `get_all()`，以列表形式返回代理池所有数据。该方法主要是内部使用，也可对外使用。  
   e. `add(proxy, response_time)`，新增一个代理，需要传入代理和响应时间。该方法目前只在内部使用到。  
   f. `add_many(proxies, response_time)`，新增多个代理，需要传入代理列表和响应时间。该接口目前提供给爬虫模块使用。  
   g. `delete(proxy)`，删除一个代理。该接口目前提供给过滤模块使用。  
   h. `_delete_all()`，删除代理池所有数据。该方法仅在开发阶段测试使用  。
   i. `__len__()`，获得代理池的总数。该方法目前仅在内部使用，也可对外使用。  

### 四. settings配置
```python
# settings.py

# 代理爬虫开关。True为开启代理爬虫，False为关闭代理爬虫
OPEN_SPIDER = True
# 过滤器开关。True为开启过滤(所有带筛选的代理)，False为关闭过滤
OPEN_FILTER = True

# 定时任务,单位(s)
# 需要注意的是，由于程序中定时任务是基于sleep设计的，会出现定时与预期不符的问题。
# 测试结果：爬虫模块耗时0.5~1s; 过滤模块耗时10.5~11s(0.5~1s程序耗时，10s为设置的代理最长响应时间)

# 爬虫模块循环时间(10.5s~11s)
SPIDER_FREQUENCY = 10
# 过滤模块循环时间(10~11s)
FILTER_FREQUENCY = 0

# 注册爬虫
# 新建爬虫时(应该不会再新建爬虫了..),需要在proxypool/spiders文件夹下新建一个爬虫文件，然后把路径写入到列表中
# 集合中的爬虫模块，会通过反射，被spider.py依次调用
INSTALLED_SPIDERS = [
    "spiders.qingting.QingTingSpider",
    # "spiders.kuai.KuaiSpider",
]

# 测试代理的网站（这里选取百度，因为稳定）
PROXY_FILTER_URL = "http://www.baidu.com/"

# 同时检测的代理并行数（异步）
PROXY_FILTER_COUNT = 20

# 代理检测超时时间。超过该时间，会被直接丢弃
PROXY_FILTER_TIMEOUT = 10

# 考虑到可能修改默认响应时间，将其单独写作一个变量
# 新爬取代理添加默认响应时间(10s)
INITIAL_RESPONSE_TIME = 10
# 代理最短响应时间(0s)
MIN_RESPONSE_TIME = 0
# 代理最长响应时间(10s)
MAX_RESPONSE_TIME = PROXY_FILTER_TIMEOUT

# redis配置（账户和密码暂时没有写）
REDIS_CONF = {
    "HOST": "127.0.0.1",
    "PORT": 6379,
    "USER": "",
    "PASSWORD": "",
    "DB": 12,
    "TABLE": "proxypool"
}

# 代理池最大数量。若后续有需要，使用该变量。
# PROXY_POOL_SIZE = 300
```

### 五. 存在问题
##### 1. `sleep()`不适合定时任务。
> 由于python程序是自上向下执行，因此程序会出现每隔15s运行一次的情况（10s程序执行时间+5s休眠时间）

考虑到前期先使用，后期再深入学习apscheduler定时模块，寻找好的解决方法。

##### 2. 需要引入`logging`模块，替换掉`print`函数

##### 3. 代理存储结构还存在没有爬取时间字段的问题
> 预想代理结构：response_time: "127.0.0.1#4000#1533239823"
目前的代理结构：response_time: "127.0.0.1#4000"

出现该问题的主要原因是不太会用（好像还需要再写一个表），有些麻烦。而且，如果过滤模块检测到一个代理不可用（不可用或者超时），直接丢弃的话，可以不考虑判断爬取时间是否超过三分钟。（当代理池规模不大时，三分钟内每个代理都会被检测多次）

##### 4. 对`aiohttp`和`asyncio`模块还不熟悉
虽然运行结果没有出问题，但需要深入学习这两个模块，保证后续使用的稳定

##### 5. 外部如何调用代理
预想的是在项目中新增一个文件`proxypool.py`，该文件直接从redis中取代理，然后封装一些方法。
在爬虫项目的中间件中，通过调用`proxypool.py`中的方法，实现从代理池中取出代理。

##### 6. 个人感觉sleep不稳定(需要根据运行情况改进)

##### 7. 异步检测，并发数不确定
程序运行时间约为0.5s\~1s，sleep设置为10s。每天能获取到7854\~8228(8000)
假设过滤器每10s运行一次，每次并发数为20。代理池平均数量为60\~180。则新代理最晚刷新响应时间为30~90s

##### 8. redis分区选择哪个，表名称如何使用尚不确定

##### 9. 代理字段问题
如果未来有需要，需要添加的字段有：
> IP（已有）
> port（已有）
> 响应时间（已有）
> 爬取时间（暂未添加）
> 地区
> 透明度
> 支持协议
> 白名单
> 其他

##### 10. 代理响应时间不精确
由于aiohttp的response没有获取响应时间的方法，暂时使用了记录开始时间，记录收到响应时间，相减得到响应时间。

### 六. 代码仓库
```python
http://www.baidu.com
```

