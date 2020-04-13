·

#### 数据库分类
RDBMS: MySQL、Oracle、SQL Server. 适用于数据关系复杂或数据安全级别较高的场景。 
NoSQL: Mongodb、Redis. 适用于高性能数据存储，在生产中大多起辅助RDBMS作用。

#### MySQL安装
```bash shell
# 1.下载mysql5.7.20软件包
yum install -y libaio-devel


# 2.移动到执行目录下
mv mysql-5.7.20-linux-glibc2.12-x86_64 /app/mysql

# 3.将mysql/bin添加进环境变量
vim /etc/profile
export PATH=/app/mysql/bin:$PATH
source /etc/profile

# 3.建立mysql用户和组
useradd mysql

# 4.创建mysql相关目录，并修改权限
# /app/mysql/: mysql程序位置
# /data/mysql/: mysql数据位置
mkdir /data/mysql -p
chown -R mysql.mysql /app/*
chown -R mysql.mysql /data/*

# 5.初始化数据库
# 方式一：（不推荐）初始化数据，初始化管理员的临时密码
# 5.7开始,MySQL加入了全新的 密码的安全机制:
# 初始化完成后,会生成临时密码(显示到屏幕上,并且会往日志中记一份)
# 密码复杂度:长度:超过12位? 复杂度:字符混乱组合
# 密码过期时间180天
mysqld --initialize  --user=mysql --basedir=/app/mysql --datadir=/data/mysql

# 方式二：（推荐，会跳过新加入的密码安全机制）初始化数据，初始化管理员的密码为空
mysqld --initialize-insecure  --user=mysql --basedir=/app/mysql --datadir=/data/mysql

# 方式三：只适用于5.6版本mysql的初始化
/application/mysql/scripts/mysql_install_db  --user=mysql --datadir=/application/mysql/data --basedir=/application/mysql

# 6.编写配置文件
vim /etc/my.cnf

[mysqld]
user=mysql
basedir=/app/mysql
datadir=/data/mysql
server_id=6
port=3306
socket=/tmp/mysql.sock
[mysql]
socket=/tmp/mysql.sock
prompt=3306 [\\d]>

# 7.配置启动脚本
cp /app/mysql/support-files/mysql.server /etc/init.d/mysqld

# 8.使用systemd管理mysql
# systemctl  start/stop/restart/status   mysqld
vim /etc/systemd/system/mysqld.service
[Unit]
Description=MySQL Server
Documentation=man:mysqld(8)
Documentation=http://dev.mysql.com/doc/refman/en/using-systemd.html
After=network.target
After=syslog.target
[Install]
WantedBy=multi-user.target
[Service]
User=mysql
Group=mysql
ExecStart=/app/mysql/bin/mysqld --defaults-file=/etc/my.cnf
LimitNOFILE = 5000


```


