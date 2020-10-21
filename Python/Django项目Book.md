## Django项目Book

```bash
#django版本和python版本的对应

Django version	       python versions
 
1.8	               2.7,3.2,3.3,3.4,3.5
1.9,1.10	       2.7,3.4,3.5
1.11	           2.7,3.4,3.5,3.6
2.0	               3.4,3.5,3.6
2.1,2.2            3.5,3.6,3.7
3.0,3.1            3.6,3.7,3.8   
```



Python版本：python 3.6.9

Django版本：django 2.1.8



### 1 配置环境

```bash
mkdir -p /opt/Projects/book
cd /opt/Projects/book
python -m venv venv		# 创建虚拟环境
source venv/bin/activate	# 激活虚拟环境
(venv) [test@test book]$ pip install django==2.1.8		# 安装指定版本django
```

```bash
(venv) [test@test book]$ django-admin startproject books .	# 创建项目
(venv) [test@test book]$ python manage.py startapp bookapp	# 创建应用程序
(venv) [test@test book]$ python manage.py migrate	# 创建数据库
```



### 2 添加应用

```bash
# django添加应用bookapp
books/settings.py
```

```python
INSTALLED_APPS = (
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'bookapp',
)
```

```bash
LANGUAGE_CODE = 'zh-hans'
TIME_ZONE = 'Asia/Shanghai'
```



### 3 模型设计

```bash
# 设计网站模型
bookapp/models.py
```

```python
#coding=utf-8
from django.db import models

class BookInfo(models.Model):
    btitle = models.CharField(max_length=20)
    bpub_date = models.DateField()
    def __str__(self):
        return self.btitle

class HeroInfo(models.Model):
    hname = models.CharField(max_length=20)
    hgender = models.BooleanField()
    hcomment = models.CharField(max_length=100)
    hbook = models.ForeignKey('BookInfo',on_delete=models.CASCADE,)
    def __str__(self):
        return self.hname
```



### 4 迁移数据

```bash
(venv) [test@test book]$ python manage.py makemigrations
(venv) [test@test book]$ python manage.py migrate
```



### 5 后台管理

```bash
# 创建管理员
(venv) [test@test book]$ python manage.py createsuperuser
```

```bash
# 后台注册
bookapp/admin.py
```

```python
from django.contrib import admin
from bookapp.models import BookInfo,HeroInfo

class BookInfoAdmin(admin.ModelAdmin):
    list_display = ['id', 'btitle', 'bpub_date']
class HeroInfoAdmin(admin.ModelAdmin):
    list_display = ['id', 'hname','hgender','hcomment']

admin.site.register(BookInfo,BookInfoAdmin)
admin.site.register(HeroInfo,HeroInfoAdmin)
```



### 6 更新视图

```bash
# 更新视图
bookapp/views.py
```

```python
#coding=utf-8
from django.shortcuts import render
from bookapp.models import BookInfo

#首页，展示所有图书
def index(reqeust):
    #查询所有图书
    booklist = BookInfo.objects.all()
    #将图书列表传递到模板中，然后渲染模板
    return render(reqeust, 'bookapp/index.html', {'booklist': booklist})

#详细页，接收图书的编号，根据编号查询，再通过关系找到本图书的所有英雄并展示
def detail(reqeust, bid):
    #根据图书编号对应图书
    book = BookInfo.objects.get(id=int(bid))
    #查找book图书中的所有英雄信息
    heros = book.heroinfo_set.all()
    #将图书信息传递到模板中，然后渲染模板
    return render(reqeust, 'bookapp/detail.html', {'book':book,'heros':heros})
```



### 7 更新URL

```bash
# 更新books目录URL
books/urls.py
```

```python
from django.contrib import admin
from django.urls import path
from django.conf.urls import include, url

urlpatterns = [
    path('admin/', admin.site.urls),
    url(r'^', include('bookapp.urls')),
]
```



```bash
# 更新bookapp目录URL
bookapp/urls.py
```

```python
#coding=utf-8
from django.conf.urls import url
#引入视图模块
from bookapp import views
urlpatterns = [
    #配置首页url
    url(r'^$', views.index),
    #配置详细页url，\d+表示多个数字，小括号用于取值，建议复习下正则表达式
    url(r'^(\d+)/$',views.detail),
]
```



### 8 更新模板

```bash
# 添加模板路径
books/settings.py
```

```python
'DIRS': [os.path.join(BASE_DIR, 'templates')],
```



```bash
# 创建首页模板
templates/bookapp/index.html
```

```python
<html>
<head>
    <title>首页</title>
</head>
<body>
<h1>图书列表</h1>
<ul>
    {#遍历图书列表#}
    {%for book in booklist%}
    <li>
     {#输出图书名称，并设置超链接，链接地址是一个数字#}
      <a href="/{{book.id}}/">{{book.btitle}}</a>
    </li>
    {%endfor%}
</ul>
</body>
</html>
```



```bash
# 创建内容模板
templates/bookapp/detail.html
```

```html
<html>
<head>
    <title>详细页</title>
</head>
<body>
<h1>{{book.btitle}}</h1>
<ul>
    {%for hero in heros%}
    	<li>{{hero.hname}}---{{hero.hcomment}}</li>
    {%endfor%}
</ul>
</body>
</html>
```



