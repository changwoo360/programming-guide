### time
```python
['_STRUCT_TM_ITEMS', '__doc__', '__loader__', '__name__', '__package__', '__spec__', 'altzone', 'asctime', 'clock', 'ctime', 'daylight', 'get_clock_info', 'gmtime', 'localtime', 'mktime', 'monotonic', 'monotonic_ns', 'perf_counter', 'perf_counter_ns', 'process_time', 'process_time_ns', 'sleep', 'strftime', 'strptime', 'struct_time', 'time', 'time_ns', 'timezone', 'tzname', 'tzset']
```
name | 功能 |  其他  
-|-|-
time() | 时间获取 | 5 |
gmtime() | 时间获取 | 6 |
ctime() | 时间获取 | 7 |
asctime() | 时间获取 | 7 |
localtime() | 时间获取 | 7 |
mktime() | 时间获取 | 7 |
-|-|-
strftime() | 时间格式化 | 7 |
strptime() | 时间格式化 | 7 |

字符串时间转换为时间戳
```python
import time

today = str(time.strftime("%Y-%m-%d"))  # 2019-12-20
# 先将字符串时间变为结构化时间，再将结构化时间变为时间戳
today_mktime = time.mktime(time.strptime(today, "%Y-%m-%d"))
```

### loguru
安装
```bash
pip3 install loguru
```
基本使用
```python
from loguru import logger

logger.debug("This is a debug message.")
```
输出日志到文件中
```python
logger.add("run.log")
logger.debug("This is a debug message.")
```
retation配置
```python
# 设置日志大小阈值
logger.add("run_{time}.log", rotation="100 MB")

# 每天0点创建log文件
logger.add("run_{time}.log", rotation="00:00")

# 每隔一周创建一个log文件
logger.add("run_{time}.log", rotation="1 week")
```
retention配置
```python
# 设置日志文件最长保留10天
logger.add(run.log, retention="10 days")
```
compression配置
```python
# 配置文件的压缩格式
logger.add("run.log", compression="zip")
```
Traceback记录
```python
@logger.catch
def my_function(x, y, z):
    # An error? It's caught anyway!
    return 1 / (x + y + z)

# 加上装饰器后，如果出现错误，会自动记录
> File "run.py", line 15, in <module>
    my_function(0, 0, 0)
    └ <function my_function at 0x1171dd510>
 
  File "/private/var/py/logurutest/demo5.py", line 13, in my_function
    return 1 / (x + y + z)
                │   │   └ 0
                │   └ 0
                └ 0
 
ZeroDivisionError: division by zero
```

#### 参考文章
- [loguru官方文档](https://loguru.readthedocs.io/en/stable/index.html)
- [Python 中更优雅的日志记录方案 loguru](https://cuiqingcai.com/7776.html)
