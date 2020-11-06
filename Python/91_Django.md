## Django

```bash
#django版本和python版本的对应
Django version	   python versions
1.8	               2.7,3.2,3.3,3.4,3.5
1.9,1.10	       2.7,3.4,3.5
1.11	           2.7,3.4,3.5,3.6
2.0	               3.4,3.5,3.6
2.1,2.2            3.5,3.6,3.7
3.0,3.1            3.6,3.7,3.8   
```

Python版本：python 3.6.9

Django版本：django 2.1.8



## 一 Notes项目

#### 1 创建项目

```bash
mkdir -p /opt/Projects/notes
cd /opt/Projects/notes
python -m venv venv		# 创建虚拟环境
source venv/bin/activate	# 激活虚拟环境
deactivate		# 退出虚拟环境
```

```bash
pip install django==2.2.2			# 安装指定版本django
django-admin startproject notes .	# 创建项目
python manage.py startapp web		# 创建应用程序
```



#### 2 配置文件(settings.py)

notes/settings.py

```python
#coding=utf-8

ALLOWED_HOSTS = ["*"]

INSTALLED_APPS = (
    --snip--
    'bookapp',	# 添加应用程序
)

TEMPLATES = [
    {
        --snip--
        'DIRS': [os.path.join(BASE_DIR, '')],	# 添加网页模板路径
        --snip--
    },
]

# 更改数据库信息
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': 'book',
        'USER': 'book',
        'PASSWORD': '123456',
        'HOST': '192.168.100.11',
        'PORT': '3306',
    }
}

LANGUAGE_CODE = 'zh-hans'	# 更改显示语言

TIME_ZONE = 'Asia/Shanghai'
```



#### 3 运行项目

```bash
# 测试项目
python manage.py runserver 192.168.100.11:8000
```



#### 4 模型设计(models.py)

web/models.py

```python
#coding=utf-8
from django.db import models

class Topic(models.Model):
	"""笔记主题"""
	topic_name = models.CharField(max_length=200)	# 200个字符
	ctime = models.DateTimeField(auto_now_add=True)	# 自动设置成当前日期和时间
	def __str__(self):	# 显示有关主题的信息
		return self.topic_name

class Entry(models.Model):
	"""笔记内容"""
	topic = models.ForeignKey(Topic,on_delete=models.CASCADE,)	# 外键
	text = models.TextField()
	ctime = models.DateTimeField(auto_now_add=True)
	class Meta:	# 使用entries表示多个条目
		verbose_name_plural = 'entries'
	def __str__(self):
		return self.text[:50] + "..."
```

```bash
# 模型数据写数据库
python manage.py makemigrations web
python manage.py migrate
```



#### 5 后台管理(admin.py)

```bash
# 创建管理员
python manage.py createsuperuser
```

web/admin.py

```python
#coding=utf-8
from django.contrib import admin
from learning_logs.models import Topic，Entry

class TopicAdmin(admin.ModelAdmin):
    list_display = ['id','topic_name','ctime']
class EntryAdmin(admin.ModelAdmin):
    list_display = ['id', 'text','ctime']

admin.site.register(Topic,TopicAdmin)
admin.site.register(Entry,EntryAdmin)
```

浏览器访问：http://192.168.100.11:8000/admin/

添加主题和内容



#### 6 映射URL(urls.py)

1】books/urls.py

```python
from django.contrib import admin
from django.urls import path
from django.conf.urls import include, url

urlpatterns = [
    path('admin/', admin.site.urls),
    url(r'^', include('bookapp.urls')),
]
```



2】web/urls.py

```python
#coding=utf-8
from django.conf.urls import url
from booktest import views
urlpatterns = [
    url(r'^$', views.index),
    url(r'^(\d+)/$',views.detail),
]
```





#### 7 编写视图(views.py)

web/views.py

```python
#coding=utf-8
from django.shortcuts import render
from booktest.models import BookInfo

#首页，展示所有图书
def index(reqeust):
    #查询所有图书
    booklist = BookInfo.objects.all()
    #将图书列表传递到模板中，然后渲染模板
    return render(reqeust, 'booktest/index.html', {'booklist': booklist})

#详细页，接收图书的编号，根据编号查询，再通过关系找到本图书的所有英雄并展示
def detail(reqeust, bid):
    #根据图书编号对应图书
    book = BookInfo.objects.get(id=int(bid))
    #查找book图书中的所有英雄信息
    heros = book.heroinfo_set.all()
    #将图书信息传递到模板中，然后渲染模板
    return render(reqeust, 'booktest/detail.html', {'book':book,'heros':heros})
```



#### 8 编写网页















