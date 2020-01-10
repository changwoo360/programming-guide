### 远程连接mysql失败
> 当mysql搭建在服务器上，远程连接容易出现连接不上的问题。  

解决方法：  
一、检查django的settings.py中数据库配置是否有误  
二、查看服务器端口开放情况，mysql默认需要开放入方向3306端口  
三、对数据库进行授权  
```bash
grant all privileges on *.* to root@"%" identified by "changwoo（远程登录密码）";   # %表示所有主机
flush privileges;  # 刷新
```
参考文章：  
* [解决django项目无法连接远程mysql的问题 - zengjielin - 博客园](https://www.cnblogs.com/zengjielin/p/8582826.html)
* [django项目连接远程mysql数据库](https://blog.csdn.net/weixin_31955923/article/details/80086135)
* [Navicat 连接 Mysql 出现1251错误 - SUNbrightness的博客 - CSDN博客](https://blog.csdn.net/SUNbrightness/article/details/80600953)

### 部署django项目后修改代码不生效
> 这里主要是在线上环境，我们上传代码后，发现没有生效，因为会生成.pyc文件  

```bash
# 删除日记，方便重启后查看有没有报错，非必须
rm -rf uwsgi.log

# 关闭所有 uwsgi进程，发现通过uwsgi --rolad ****.pid 不一定有用
killall -9 uwsgi

# 启动 uwsgi
uwsgi --ini /home/wwwroot/file.***.com/wandehua/uwsgi.ini

# 重启一下nginx
service nginx reload
```








