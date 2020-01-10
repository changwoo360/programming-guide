### MySQL 基本结构
> 在初次安装mysql时，一定要记住mysql的安装路径  
```text
# mysql/bin/ 目录结构
.
├── mysql
├── mysql.server -> ../support-files/mysql.server
├── mysqladmin
├── mysqlbinlog
├── mysqlcheck
├── mysqld
├── mysqld_multi
├── mysqld_safe
├── mysqldump
├── mysqlimport
├── mysqlpump
... (省略其他文件)
```
服务器可执行文件 | 作用
- | -
mysqld | 直接启动一个mysql服务器进程，不常用
mysqld_safe | 间接调用mysqld，会启动一个监控进程，帮助服务器进程重启，会产生错误日志
mysql.server | 间接调用mysqld_safe，可以使用`mysql.server start`和`mysql.server stop`
mysqld_multi | 可以对多个mysql服务器进程的启动和停止进行监控。

> mysqld -P3307  
> mysql -uroot -ppassword -hlocalhost -P3307 

### mysql架构
![image_1c8d26fmg1af0ms81cpc7gm8lv39.png-97.9kB](https://user-gold-cdn.xitu.io/2018/12/28/167f4c7b99f87e1c?w=842&h=559&f=png&s=100231)

#### 连接管理
客户端进程可以采用`TCP/IP`（服务端进程和客户端进程不在同一台主机）、
`命名管道或共享内存`（windows用户）、`Unix域套接字`（服务端进程和客户端进程运行在同一台操作系统上）
几种方式与服务器进程建立连接。  
每当有一个客户端进程连接到服务端进程时，服务器进程都会创建一个线程来专门处理与这个客户端的交互，
当该客户端退出时会与服务器断开连接，服务器并不会**立即把与该客户端交互的线程销毁掉**，
而是把它缓存起来。在另一个新的客户端再进行连接时，把这个缓存的线程分配给新客户端。
这样就起到了不频繁创建和销毁线程的效果，从而节省开销。  
> MySQL服务器会为每一个连接进来的客户端分配一个线程，但是线程分配太多的话会严重影响系统性能，
因此也要限制一下可以同时连接到服务器的客户端数量。  

#### 查询缓存
mysql服务端程序会将查询请求和结果缓存起来，这个查询缓存可以在不同的客户端之间共享。  
如果两个查询请求在任何字符上有不同，都会导致缓存不会命中；  
如果查询请求中包含系统函数（NOW）、自定义变量和系统表（infomation_schema），请求不会被缓存；  
如果该表的结构或数据被修改，那使用该表的所有高速缓存查询都将变为无效。
> 虽然查询缓存有时可以提升系统性能，但也不得不因维护这块缓存而造成一些开销，比如每次都要去查询缓存中检索，查询请求处理完需要更新查询缓存，维护该查询缓存对应的内存区域。从MySQL 5.7.20开始，不推荐使用查询缓存，并在MySQL 8.0中删除。  

#### 语法解析
mysql服务器程序会对客户端程序发送的请求文本进行判断解析，从文本中提取出要查询的表和各种查询字段

#### 查询优化
mysql的优化程序会将我们的查询语句优化，如外连接转为内连接、表达式转换等等。  
优化的结果是生成一个执行计划，这个执行计划表明了应该使用那些索引进行查询，表之间的连接顺序是什么样子。 可以使用`EXPLAIN`语句来查看某个语句的执行计划。 
#### 存储引擎
mysql服务器将数据的存储和提取操作都封装到了一个叫存储引擎的模块里。  
为了实现不同的功能，mysql提供了许多存储引擎，不同存储引擎管理的表具体的存储结构可能不同，
采取的存取算法也可能不同。  
> 存储引擎以前叫做**表处理器**。它的功能就是接收上层传下来的指令，然后对表中的数据进行提取或写入操作。  
> 为了管理方便，人们把连接管理、查询缓存、语法解析、查询优化这些并不涉及真实数据存储的功能划分为MySQL server的功能，
把真实存取数据的功能划分为存储引擎的功能。各种不同的存储引擎向上边的MySQL server层提供统一的调用接口
（也就是存储引擎API），包含了几十个底层函数，像"读取索引第一条内容"、"读取索引下一条内容"、
"插入记录"等等。  

### 存储引擎
存储引擎 | 描述 | 特点
- | - | -
InnoDB |（mysql默认存储引擎）具备外键支持功能的事务存储引擎 | 占用空间大，处理速度慢
MyISAM | 主要的非事务处理存储引擎 | 占用空间小，处理速度快
MEMORY | 置于内存的表 | 速度快，断点不保存，安全性低
ARCHIVE | 用于数据存档（行被插入后不能再修改）
BLACKHOLE | 丢弃写操作，读操作会返回空内容
CSV | 在存储数据时，以逗号分隔各个数据项
FEDERATED | 用来访问远程表
MERGE | 用来管理多个MyISAM表构成的表集合
NDB | MySQL集群专用存储引擎  

#### 引擎相关语句
```mysql
# 创建表时执行引擎
create table demo_table(i int) engine=MyISAM;

# 修改表的存储引擎
alter table demo_table engine=MEMORY

# 查看所有的引擎
show engines;

# 查看表结构
show create table demo_table;
```

### pymysql模块
#### 连接和关闭
```python
import pymysql


def pymysql_query_demo():
    conn = pymysql.connect(
        host="localhost",
        user="user",
        password="password",
        database="database",
        port=3306
    )

    # type of result is tuple.
    cursor = conn.cursor()

    # type of result is dict.
    cursor = conn.cursor(cursor=pymysql.cursors.DictCursor)

    # 增删改查
    ...

    cursor.close()
    conn.close()
```
#### 增删改查
```python
def pymysql_CRUD_demo(cursor, conn):
    """Change data must use commit method."""
    # query one
    cursor.execute("select Title from database where tag=1")
    result = cursor.fetchone()

    # query 10
    cursor.execute("select Title from database where tag=1")
    result = cursor.fetchmany(10)

    # query all
    cursor.execute("select Title from database where tag=1")
    result = cursor.fetchall()

    # insert one 1
    cursor.execute("insert into table(id, name) values(1, 'world')")
    conn.commit()

    # insert one 2
    sql = "insert into table(id, name) values(%s, %s)"
    cursor.executemany(sql, [1, "world"])
    conn.commit()

    # insert many
    sql = "insert into table(id, name) values(%s, %s)"
    cursor.executemany(sql, [(1, "world1"), (2, "world2")])
    conn.commit()

    # update one
    sql = "update table set compress_path='compress_path' where file_path='file_path'"
    cursor.execute(sql)
    conn.commit()
```
#### pymysql注入问题
```python
# 存在bug的sql语句
sql = "SELECT * FROM chart1 where username='%s' and password='%s'"%(user,pwd)
cursor.execute(sql)
```
> 直接用字符串拼接sql语句会出现sql注入问题。如果用户输入了`'`、`"`、`--`，会导致sql语句被截断，不用登陆即可成功。


### mysql复杂语句
查询符合条件的当天的数据量
```sql
select DomainName, count(1) from spd_metadata where domain in ("www.zhuwe.cn", "xin.baidu.com", "www.ccgp.gov.cn") and to_days(CrawledTime) = to_days(now()) GROUP BY DomainName
```
查询符合条件的昨天的数据量
```sql
select DomainName, count(1) from spd_metadata where domain in ("www.zhuwe.cn", "xin.baidu.com", "www.ccgp.gov.cn") and TO_DAYS(NOW()) - TO_DAYS(CrawledTime) <= 1 and Parenthashcode is null GROUP BY DomainName
```
查询某个网站下所有的菜单数据
```sql
SELECT Domain,ChannelLevel1,ChannelLevel2,ChannelLevel3,ChannelLevel4,COUNT(1) from spd_metadata where Domain="www.sxggzyjy.cn" GROUP BY Domain, ChannelLevel1,ChannelLevel2,ChannelLevel3,ChannelLevel4,ChannelLevel5,ChannelLevel6
```

### 参考文章
- [Ubuntu 安装mysql和简单操作 - KingsLanding - 博客园](https://www.cnblogs.com/zhuyp1015/p/3561470.html)
- [mysql5.7初始化密码报错  ERROR 1820](https://blog.csdn.net/memory6364/article/details/82426052)
- [Python开发: MySQL - 吴沛齐](https://www.cnblogs.com/wupeiqi/articles/5713315.html)
