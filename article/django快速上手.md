#### 一、安装配置

##### 1. 安装django
```python
pip3 install django==1.11.1 -i https://pypi.douban.com/simple
```

##### 2. 安装rest-framework
```python
pip3 install djangorestframework
```

##### 3. 安装pymysql
```python
pip3 install pymysql
```

#### 二、简单使用
##### 1. 使用django初始化一套模板（和scrapy的startproject一样）
```python
# 使用django创建项目
django-admin startproject log_api

# 进入创建的项目
cd log_api

# 创建一个app
django-admin startapp monitoring

# 在settings.py中注册该app
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'rest_framework',  # 注册djangorestframework组件
    'monitoring.apps.MonitoringConfig',  # 注册新创建的app
]
```
```
# 项目结构如下：

├── log_api
│   ├── __init__.py
│   ├── settings.py    <---配置文件
│   ├── urls.py        <---路由文件（url和view函数对应）
│   └── wsgi.py
├── manage.py
├── monitoring
│   ├── __init__.py
│   ├── admin.py
│   ├── apps.py
│   ├── migrations
│   │   └── __init__.py
│   ├── models.py      <---models文件（用于和数据库关联）
│   ├── tests.py
│   └── views.py       <---视图文件（处理主要逻辑）
```

##### 2. 配置mysql连接
```python
# log_api/settings.py

DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',  # mysql引擎
        'HOST': '127.0.0.1',
        'PORT': '3306',
        'USER': 'root',
        'PASSWORD': '',  # mysql密码
        'NAME': 'log_monitoring',  # 要使用的数据库名称
    }
}
```
```python
# log_api/__init__.py

import pymysql

pymysql.install_as_MySQLdb()
```

##### 3. 使用model生成数据表
```python
# monitoring/models.py

from django.db import models

# 添加一个新的数据表
class SpiderLog(models.Model):
    title = models.CharField(max_length=128)  # 广州爬虫spider_1
    crawl_time = models.DateField(auto_created=True, auto_now_add=True)  # 爬虫运行时间
    file_path = models.CharField(max_length=255)  # 爬虫日志文件路径

    class Meta:
        db_table = "spider_log"  # 使用自己定义的名称，避免django自动在数据库创建其规则的名称
```
```python
# 修改models.py后，要执行两条命令使其生效
python3 manage.py makemigrations
python manage.py migrate
```

##### 3. 开始写api
3.1 首先要写好路由对应关系
```python
# log_api/urls.py

url(r'spider_log/$', views.SpiderLog.as_view({'get': 'list'})),
```

3.2 写对应的视图函数
```python
from monitoring import models
from rest_framework import serializers  # 序列号组件
from rest_framework import viewsets  # API接口


class SpiderLogModelSerializer(serializers.ModelSerializer):
    class Meta:
        model = models.SpiderLogModel
        # fields = ["id", "title"]
        fields = "__all__"


class SpiderLog(viewsets.ModelViewSet):
    queryset = models.SpiderLogModel.objects.all()
    serializer_class = SpiderLogModelSerializer  # 序列化组件
```
> 这样就可以实现一个简单的api了。但实际使用复杂的多，比如实现修改字段信息、api信息中输出一对一或多对多字段信息、接口规范、接口版本控制、分页、访问频率、携带token等等。这些在restframework中都可以实现。  

#### 三、几个重要的问题

##### 3.1 数据库，各种表结构已经创建好了，甚至连数据都有了，此时，我要用Django管理这个数据库，ORM映射怎么办？
Django是最适合所谓的green-field开发，即从头开始一个新的项目。但是呢，Django也支持和以前遗留的数据库和应用相结合的
```python
# 根据数据库表，生成对应的models.py
python manage.py inspectdb > models.py

# 根据需要，删除部分表，或者修改managed
managed = True  # 允许django对mysql进行修改

# 更新
python manage.py migrate
```

##### 3.2 如何获取到不同的queryset对象
示例视图类中一共有两行代码，一行是得到包含数据的queryset对象，另一行是为其选用一个序列化组件。由于序列化组件仅仅只是对数据进行简单处理，因此如何获取到不同的queryset对象，就能为api提供不同的数据。
```python

# 获取所有数据
objects.all()

# 获取筛选后的数据
objects.filter("is_crawled"=1)

# 获取排序后的数据
objects.all().order_by("id")  # 根据id排序

# 排序反转
objects.all().reverse()

# 获取id大于1，小于10的数据
objects.filter(id__gt=1, id__lt=10)

# 获取id在[1,2,3]中的数据
objects.filter(id__in=[1,2,3])

# 获取爬取时间在2019年的数据
objects.filter(crawled_time__year="2019")
```

##### 3.3 序列化组件又该如何写呢？

```python
class SpiderLogModelSerializer(serializers.ModelSerializer):
    class Meta:
        model = models.SpiderLogModel
        # fields = "__all__"
        # 可以自定义字段
        fields = ["id", "title", "crawled_time", "file_path"]
        # 可以自定义一对多深度（自己处理也可以）
        depth = 1
```
##### 3.5 路由
```python

url(r'web/api/(?P<pk>\d+)/$', views.ArticleAPI.as_view({'get': 'retrieve', 'delete': 'destroy', 'put': 'update', 'patch': 'partial_update'})),

```



##### 3.4 如何增加一个自定义字段呢？
pass


#### 四、Nginx+uwsgi部署Django
##### 4.1 环境准备
> Centos7（也可以选择其他Linux系统）
> python3
> pip3
> MySQL
> Nginx
> uwsgi
> djagno==1.11.1
> restframework
```python
# 安装uwsgi（两种方式）
yum install uwsgi
pip3 install uwsgi

# 安装restframework
pip3 install djangorestframework
```

##### 4.2 测试uwsgi是否可用
```python
# 在项目目录创建xml文件（其他格式也可以,是uwsgi的参数）
# mysite.xml
<uwsgi>    
   <socket>127.0.0.1:8997</socket><!-- 内部端口，自定义 --> 
   <chdir>/data/wwwroot/mysite/</chdir><!-- 项目路径 -->            
   <module>mysite.wsgi</module> 
   <processes>4</processes> <!-- 进程数 -->     
   <daemonize>uwsgi.log</daemonize><!-- 日志文件 -->
</uwsgi>

# 测试配置是否生效
uwsgi mysite.xml
```

##### 4.3 配置Nginx
```vi /etc/nginx/nginx.conf```
```python
worker_processes  1;
events {
    worker_connections  1024;
}
http {
    include       mime.types;
    default_type  application/octet-stream;
    sendfile        on;
    server {
        listen       80;
        server_name  www.django.cn;
        charset utf-8;
        location / {
           include uwsgi_params;
           uwsgi_pass 127.0.0.1:8997;
           uwsgi_param UWSGI_SCRIPT mysite.wsgi;
           uwsgi_param UWSGI_CHDIR /data/wwwroot/mysite;
           
        }
        location /static/ {
        alias data/wwwroot/mysite/static/; 
        }
    }
}
```

##### 4.4 重启Nginx
```systemctl restart nginx```










