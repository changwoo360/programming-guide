## yum 配置
```shell script
#!/bin/bash

cd /etc/yum.repos.d/
mkdir allbak
mv ./*.repo allbak
printf "已将默认的yum源移除，并移至/etc/yum.repos.d/allbak/目录下\n"

wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo

yum clean all
yum makecache

wget -O /etc/yum.repos.d/epel.repo http://mirrors.aliyun.com/repo/epel-7.repo

printf "yum配置成功!"
```