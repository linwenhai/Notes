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



### 1 learning_log项目

#### 1.1 创建项目

```bash
mkdir -p /opt/Projects/learning_log
cd /opt/Projects/learning_log
python -m venv venv		# 创建虚拟环境
source venv/bin/activate	# 激活虚拟环境
deactivate		# 退出虚拟环境
```



```bash
pip install django==2.1.8		# 安装指定版本django
django-admin startproject learning_log	# 创建项目
python manage.py startapp learning_logs	# 创建应用程序
python manage.py migrate	# 创建数据库
```



```bash
/opt/Project/learning_log/learning_log/settings.py
```

```python
INSTALLED_APPS = (
    ...
    'learning_logs',	# 添加应用程序
)
```

```bash
LANGUAGE_CODE = 'zh-hans'	# 更改显示语言
TIME_ZONE = 'Asia/Shanghai'
```

```python
ALLOWED_HOSTS = ["*"]
```



```bash
# 测试项目
python manage.py runserver 192.168.100.11:8000
```



#### 1.2 模型设计

```bash
/opt/Project/learning_log/learning_logs/models.py
```

```python
#coding=utf-8
from django.db import models
class Topic(models.Model):
	"""用户学习的主题"""
	text = models.CharField(max_length=200)
	date_added = models.DateTimeField(auto_now_add=True)
	def __str__(self):
		"""返回模型的字符串表示"""
		return self.text

class Entry(models.Model):
	"""学到的有关某个主题的具体知识"""
	topic = models.ForeignKey(Topic,on_delete=models.CASCADE,)
	text = models.TextField()
	date_added = models.DateTimeField(auto_now_add=True)
	class Meta:
		verbose_name_plural = 'entries'
	def __str__(self):
		"""返回模型的字符串表示"""
		return self.text[:50] + "..."
```



```bash
# 模型数据写数据库
python manage.py makemigrations learning_logs
python manage.py migrate
```



#### 1.3 后台管理

```bash
# 创建管理员
python manage.py createsuperuser
```

```bash
# 后台注册
/opt/Project/learning_log/learning_logs/admin.py
```

```python
from django.contrib import admin
from learning_logs.models import Topic，Entry
admin.site.register(Topic)
admin.site.register(Entry)
```

浏览器访问：http://192.168.100.11:8000/admin/

添加主题Chess和Rock Climbing



#### 1.4 创建网页

1】主页URL











#### 1.4 视图

```bash
# 更新视图
/opt/Project/test1/booktest/views.py
```

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



#### 1.5 URL

```bash
# 更新books目录URL
/opt/Project/test1/test1/urls.py
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
/opt/Project/test1/booktest/urls.py
```

```python
#coding=utf-8
from django.conf.urls import url
from booktest import views
urlpatterns = [
    url(r'^$', views.index),
    url(r'^(\d+)/$',views.detail),
]
```



#### 1.6 模板

```bash
# 添加模板路径
/opt/Project/test1/test1/settings.py
```

```python
'DIRS': [os.path.join(BASE_DIR, 'templates')],
```



```bash
# 创建首页模板
/opt/Project/test1/templates/booktest/index.html
```

```python
<html>
<head>
    <title>首页</title>
</head>
<body>
<h1>图书列表</h1>
<ul>
    {%for book in booklist%}
    <li>
      <a href="/{{book.id}}/">{{book.btitle}}</a>
    </li>
    {%endfor%}
</ul>
</body>
</html>
```



```bash
# 创建内容模板
/opt/Project/test1/templates/bookapp/detail.html
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





