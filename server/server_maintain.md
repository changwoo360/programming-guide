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
export WORKON_HOME=~/Envs   #设置virtualenv的统一管理目录
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
# 安装
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
