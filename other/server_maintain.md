### 配置yum源
步骤一: 移除默认的yum源
```bash
# 进入yum配置地址
cd /etc/yum.repos.d/

# 创建一个空文件，用于备份默认的yum源
mkdir allback
mv ./* allbak
```
步骤二: 配置自定义yum源
```bash
# 使用阿里云的yum源，-O 为指定一个路径并且重命名
wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo
```
步骤三: 清除旧的yum缓存
```bash
# 清除旧的yum缓存
yum clean all

# 生成新的yum缓存
yum makecache
```
步骤四: 配置额外仓库源
```bash
# 用于阿里云yum找不到时，使用epel源
wget -O /etc/yum.repos.d/epel.repo http://mirrors.aliyun.com/repo/epel-7.repo
```

### 安装Python3
步骤一: 下载python3安装包（这里版本为3.6）
```bash
# 下载python3源文件
wget https://www.python.org/ftp/python/3.6.7/Python-3.6.7.tar.xz

# 解压
xz -d Python-3.6.7.tar.xz
tar -xf Python-3.6.7.tar
```
步骤二: 安装Python3
```bash
# 安装python所需要的环境依赖（非常重要）
yum install gcc patch libffi-devel python-devel  zlib-devel bzip2-devel openssl-devel ncurses-devel sqlite-devel readline-devel tk-devel gdbm-devel db4-devel libpcap-devel xz-devel -y

cd Python-3.6.7

# 将python安装在/opt/python36/中
./configure --prefix=/opt/python36/

# 编译安装
make
make install
```
步骤三: 添加到环境变量
```bash
# 复制当前的环境变量
echo $PATH

vim /etc/profile

# 在最后一行添加环境变量
PATH=/opt/python36/bin/:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/root/bin
或者（/opt/python36/bin/:$PATH）

# 加载配置
source /etc/profile
```
步骤四: 安装pip
```bash
pip3 install --upgrade pip
```
步骤五: 默认使用python3和pip3
```bash
which python
cd /usr/bin/
cp python python_2
cp python-config python-config_2
rm python
rm python-config
cp /opt/python36/bin/python3.6 python
cp /opt/python36/bin/python3-config python-config   
```

### 安装虚拟环境
步骤一: 安装virtualenv
```bash
pip3 install virtualenv  
```
步骤二: 安装virtualenvwrapper
```bash
pip3 install virtualenvwrapper

# 配置全局变量，使得每次登陆后，就加载virtualenvwrapper.sh脚本，使其生效
vim ~/.bashrc

# 写入配置，用于虚拟环境管理virtualenvwrapper的配置
export WORKON_HOME=~/.venv   #设置virtualenv的统一管理目录
export VIRTUALENVWRAPPER_VIRTUALENV_ARGS='--no-site-packages'   #添加virtualenvwrapper的参数，生成干净隔绝的环境
export VIRTUALENVWRAPPER_PYTHON=/opt/python36/bin/python3.6     #指定python解释器
source /opt/python36/bin/virtualenvwrapper.sh #执行virtualenvwrapper安装脚本
```
步骤三: 加载配置并重新登陆
```bash
source ~/.bashrc

logout
```
步骤四: 使用虚拟环境
```bash
# 创建虚拟环境
mkvirtualenv test

# 查看虚拟环境
mlsvirtualenv

# 进入虚拟环境
workon test

# 删除虚拟环境
rmvirtualenv test

# 进入虚拟环境家目录
cdvirtualenv

# 进入虚拟环境packages中
cdsitepackages
```

### 安装MySQL
pass

### Centos7环境初始化
安装注意: 
- 分区必须存在swap分区。可以自动创建，然后修改
- 直接插网线也可以使用yum

修改网卡配置文件
```shell script
# 步骤一：修改网卡配置文件

vim /etc/sysconfig/network-scripts/ifcfg-ens33  # 不同网卡名称不一样
ONBOOT = no  # no为不自动分配ip；yes为自动分配ip

# 步骤二：重启网卡，使配置生效
systemctl restart network
```
下载图形界面
```shell script
# 步骤一：更新yum
yum upgrade -y

# 步骤二：安装桌面环境
yum -y groupinstall "X Window System"  # 安装桌面环境之前需要装这个
yum -y groupinstall "GNOME Desktop"  # 安装GNOME桌面环境

# 步骤三：重启使用
reboot  # 重启
startx  # 切换到桌面环境
```
安装必要程序
```shell script
yum install net-tools.x86_64
```

### 安装jupyter
步骤一: 安装ipython和jupyter，并进入ipython
```bash
pip install ipython
pip install jupyter

ipython  # 进入ipython命令行
```
步骤二: 查看ipython密码
```python
from IPython.lib import passwd

# 输出并保存密码root(sha1:8f80787b7be5:95e5460f0741b408baec34894314a2fa52f0ea6e)
passwd()

exit()
```
步骤三: 配置ipython密码
```bash
jupyter notebook --generate-config --allow-root
vi ~/.jupyter/jupyter_notebook_config.py

# 修改配置
c.NotebookApp.ip = '0.0.0.0'
c.NotebookApp.open_browser = False
c.NotebookApp.password = 'sha1:4d75995b2f75:c08ada6f31047fe429d0b24652f8fbc81ec32a7c'
c.NotebookApp.port = 8000
```
步骤四: 启动jupyter
```bash
jupyter notebook  --allow-root
```

### 安装nginx
```bash
yum install nginx  # 安装

systemctl start nginx  # 启动nginx

systemctl status nginx  # 查看nginx服务

netstat -tunlp | grep 80  # 查看80端口
```

### 安装redis
```bash
# 使用yum安装

yum install redis

redis-server  # 启动server

redis-cli  # 启动client

# 修改redis密码
vi /etc/redis.conf
requirepass rsf
```
```bash
# 在Mac安装

步骤一：官网（https://redis.io/）下载redis压缩包

步骤二：解压缩+移动redis至可执行目录
tar zxvf redis-4.0.10.tar.gz  # 根据下载的redis版本进行选择
mv redis-4.0.10 /usr/local/  # 应该是可执行目录

步骤三：编译安装
cd /usr/local/redis-4.0.10/  # 进入redis文件夹
sudo make test  # 测试编译（可跳过）
sudo make install  # 安装编译

步骤四：使用redis
redis-server  # 启动redis服务端
reids-cli  # 启动redis客户端


```
### 安装docker
```bash
# 安装工具包
sudo yum install -y yum-utils

# 设置远程仓库
sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo

# 安装
sudo yum install docker-ce

# 启动
sudo systemctl start docker
service docker start

# 设置开启自启动
chkconfig docker on
```
使用
```bash
# 查看docker任务
docker ps

# 
```

### docker安装splash
```bash
# 拉取镜像
sudo docker pull scrapinghub/splash

# 在8050端口启动splash
sudo docker run -it -p 8050:8050 scrapinghub/splash
```
### Centos 安装FTP服务
```bash
# 安装vsftpd
yum install vsftpd -y

# 启动服务
systemctl start vsftpd

# 设置为自启动
chkconfig vsftpd on

```
参考文章
- [ftp工具无法连接到Linux服务器](https://www.cnblogs.com/music-liang/p/11843273.html)


### Centos7部署docsify
```bash
# 步骤
# 1.上传docsify项目至服务器（略）
# 2.安装npm和docsify-cli
# 3.安装并启动docsify服务
# 4.安装nginx，并配置端口转发

# 2.安装npm和docsify-cli
cd /usr/local/src
wget https://nodejs.org/dist/v8.9.0/node-v8.9.0-linux-x64.tar.xz

tar xf node-v8.9.0-linux-x64.tar.xz
cd /usr/local
mv src/node-v8.9.0-linux-x64 node

vi ~/.bashrc
export NODE_HOME=/usr/local/node
export PATH=$NODE_HOME/bin:$PATH
source ~/.bashrc

node -v


# 3.安装并启动docsify服务
npm i docsify-cli -g
yum install screen
screen -S programming-guide
docsify serve  # 默认在本地3000端口启动服务
ctrl a + d

# 4.安装nginx，并配置端口转发
yum install nginx
vi /etc/nginx/nginx.conf

server {
      listen 80;
      server_name your_server_name; ## 这里需要写自己的服务器名称
      root /root/projects/programming-guide; ## docsify创建的目录
      set $node_port 3000;
      index index.js index.html index.htm;
      location / {
        proxy_http_version 1.1;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header Host $http_host;
        proxy_set_header X-NginX-Proxy true;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
        proxy_pass http://127.0.0.1:$node_port$request_uri;
        proxy_redirect off;
      }
    }

systemctl restart nginx
# 5.打开80端口，查看页面
```
参考文章
- [基于滴滴云搭建轻量文档网站生成工具 Docsify](http://blog.itpub.net/31559758/viewspace-2286489/)

### 安装Cockpit
```shell script
# 安装Cockpit
yum install cockpit

# 启动cockpit.socket服务
systemctl start cockpit.socket
systemctl enable --now cockpit.socket
systemctl status cockpit.socket

# 关闭防火墙
firewall-cmd --add-service=cockpit --permanent
firewall-cmd --reload

# 开放9090端口
```
参考文章：
- [如何在 CentOS 8 中安装 Cockpit Web 控制台](https://www.linuxidc.com/Linux/2019-10/161221.htm)

