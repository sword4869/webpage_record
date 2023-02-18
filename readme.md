
# 安装
```bash
pip install django
```

```bash
# 创建项目
$ django-admin startproject <project name>

$ tree
.
├── Ki                  项目的管理,启动项目,创建APP,数据管理
│   ├── asgi.py         [固定]接受异步的网络请求
│   ├── __init__.py     
│   ├── settings.py     *[关键]项目配置文件,数据库
│   ├── urls.py         *[关键]URL和函数的对应关系
│   └── wsgi.py         [固定]接受同步的网络请求
└── manage.py
```
```bash
# 创建app
$ python manage.py startapp <app name>

$ tree
.
├── app01
│   ├── admin.py            [固定]默认admin后台管理
│   ├── apps.py             [固定]app启动
│   ├── __init__.py
│   ├── migrations          [固定]数据库变更记录
│   │   └── __init__.py
│   ├── models.py           *[关键]操作数据库
│   ├── tests.py            [固定]单元测试
│   └── views.py            *[核心]url对应的函数
├── Ki
│   ├── asgi.py
│   ├── __init__.py
│   ├── settings.py
│   ├── urls.py
│   └── wsgi.py
└── manage.py

```

# 项目配置

1. 项目配置文件`Ki/settings.py`中`INSTALLED_APPS`
   
   添加App, `"app01.apps.App01Config"`即`app01/apps.py`的类名
```python
#  app01/apps.py
class App01Config(AppConfig):
    default_auto_field = "django.db.models.BigAutoField"
    name = "app01"
```
```python
# Ki/settings.py
INSTALLED_APPS = [
    "django.contrib.admin",
    "django.contrib.auth",
    "django.contrib.contenttypes",
    "django.contrib.sessions",
    "django.contrib.messages",
    "django.contrib.staticfiles",
    "app01.apps.App01Config"
]
```

# (new)一阶段

app01中创建`templates`文件夹, 创建`hello.html`. (`app01/templates/hello.html`)
```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <title>Hello</title>
  </head>
  <body>
    <p>Hello</p>
  </body>
</html>

```

编写 app01中的views.py的index()函数
```python
from django.shortcuts import render

# Create your views here.
def index(request):
    return render(request, "hello.html")
    pass
```

将函数和url配对. 修改项目配置文件`Ki/urls.py`中`urlpatterns`
```python
from django.urls import path
# 导入 app01
from app01 import views
urlpatterns = [
    # path("admin/", admin.site.urls),
    # 映射 index链接到 app01中的views.py的index()函数
    path("index/", views.index),
]
```

# 启动

```python
# runserver_plus 表示使用 https
# --cert server.crt 表示证书
$ python manage.py runserver_plus --cert server.crt 0.0.0.0:8000
```
