## web应用程序：Django



```bash
mkdir -p /opt/Projects/learning_log
cd /opt/Projects/learning_log
/opt/python/bin/python3 -m venv ll_env
source ll_env/bin/activate
```



```bash
(ll_env) [test@test learning_log]$ pip install --upgrade pip
(ll_env) [test@test learning_log]$ pip install Django==2.2.2

# 创建项目learning_log
(ll_env) [test@test learning_log]$ django-admin.py startproject learning_log .

# 安装数据库sqlite3
(ll_env) [test@test learning_log]$ python manage.py migrate

# 运行项目
(ll_env) [test@test learning_log]$ python manage.py runserver 192.168.100.11:8000
```



```
异常错误：SQLite 3.8.3 or later is required (found 3.7.17)
Centos系统自带的sqlite3版本偏低，在上面的错误提示中要求需要SQLite 3.8.3 or later
```

```bash
# 检测版本
sqlite3 --version

# 安装sqlite 3.27.2
wget https://www.sqlite.org/2019/sqlite-autoconf-3270200.tar.gz
tar -zxvf sqlite-autoconf-3270200.tar.gz
cd sqlite-autoconf-3270200
./configure --prefix=/usr/local
make && make install

# 检测版本
/usr/local/bin/sqlite3 --version
/usr/bin/sqlite3 --version

# 替换系统低版本sqlite3
mv /usr/bin/sqlite3  /usr/bin/sqlite3_old
ln -s /usr/local/bin/sqlite3   /usr/bin/sqlite3
echo "/usr/local/lib" > /etc/ld.so.conf.d/sqlite3.conf
ldconfig
sqlite3 --version
```



```bash
# 创建应用程序
(ll_env) [test@test learning_log]$ python manage.py startapp learning_logs
```



#### 1 修改配置settings.py

learning_log/settings.py

```python
ALLOWED_HOSTS = ["*"]

INSTALLED_APPS = [
    --snip--
    'learning_logs',	# 添加应用程序
]

TEMPLATES = [
    {
        --snip--
        'DIRS': [os.path.join(BASE_DIR, '')],
        --snip--
    },
]
```





#### 2 定义模型

1】learning_logs/models.py

```python
#coding=utf-8
from django.db import models

class Topic(models.Model):
    """定义主题"""
    topic_name = models.CharField(max_length=200)     # 主题最大即200个字符
    ctime = models.DateTimeField(auto_now_add=True)
    def __str__(self):
        """显示主题简介"""
        return self.topic_name

class Entry(models.Model):
    """定义主题内容"""
    topic = models.ForeignKey(Topic,on_delete=models.CASCADE,)    # 外键，与topic关联
    content = models.TextField()
    ctime = models.DateTimeField(auto_now_add=True)
    class Meta:
        """使用entries来表示多个条目"""
        verbose_name_plural = 'entries'
    def __str__(self):
        """显示内容简介"""
        return self.content[:50] + "..."
```

```bash
# 生成迁移文件
(ll_env) [test@test learning_log]$ python manage.py makemigrations learning_logs

# 修改数据库
(ll_env) [test@test learning_log]$ python manage.py migrate
```

2】learning_logs/forms.py



#### 3 管理网站admin.py

```bash
# 创建管理员
(ll_env) [test@test learning_log]$ python manage.py createsuperuser
```



向管理网站注册模型

learning_logs/admin.py

```python
#coding=utf-8
from django.contrib import admin
from learning_logs.models import Topic,Entry

admin.site.register(Topic)
admin.site.register(Entry)
```



http://192.168.100.11:8000/admin/



#### 4 Django shell

```bash
(ll_env) [test@test learning_log]$ python manage.py shell
>>> from learning_logs.models import Topic
>>> Topic.objects.all()
<QuerySet [<Topic: 笑傲江湖>, <Topic: 倚天屠龙记>]>
>>> topics = Topic.objects.all()
>>> for topic in topics:
...     print(topic.id, topic)
... 
1 笑傲江湖
2 倚天屠龙记
>>> t = Topic.objects.get(id=1)
>>> t.topic_name
'笑傲江湖'
>>> t.ctime
datetime.datetime(2020, 11, 4, 8, 58, 50, 239233, tzinfo=<UTC>)
>>> t.entry_set.all()
<QuerySet [<Entry: 《笑傲江湖》是中国现代作家金庸创作的一部长篇武侠小说，于1967年开始创作并连载于《明报》，1969...>]>
```



#### 5 创建网页

##### 5.1 映射URL

1】learning_log/urls.py

```python
#coding=utf-8
from django.contrib import admin
from django.urls import path,include

urlpatterns = [
    # 后台管理跳转
    path('admin/', admin.site.urls),
    # 首页跳转
    path('',include('learning_logs.urls', namespace='learning_logs')),
]
```

2】learning_logs/urls.py

```python
#coding=utf-8
from django.urls import path
from . import views

app_name ='[learning_logs]'
urlpatterns = [
    # 首页
    path('',views.index, name='index'),
    # 主题网页
    path('topics/',views.topics,name='topics'),
    # 内容网页
    path('topics/<topic_id>',views.topic,name='topic'),
]
```

##### 5.2 编辑视图

learning_logs/views.py

```python
#coding=utf-8
from django.shortcuts import render
from .models import Topic

def index(request):
    """主页"""
    return render(request, 'html/index.html')

def topics(request):
    """图书主题"""
    topics = Topic.objects.order_by('ctime')    # 查询数据库Topic对象,按ctime排序
    context = {'topics': topics}    # 们定义了一个将要发送给模板的字典
    return render(request, 'html/topics.html', context) # 将变量context传递给render()

def topic(request, topic_id):
    """图书简介"""
    topic = Topic.objects.get(id=topic_id)  # 使用get()来获取主题
    entries = topic.entry_set.order_by('-ctime')   # 获取与该主题相关联的条目,排序
    context = {'topic': topic, 'entries': entries}
    return render(request, 'html/topic.html', context)
```

##### 5.3 网页

1】html/base.html（父模板）

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>图书</title>
</head>
<body>
<!--设置超链接-->
<a href="{% url 'learning_logs:index' %}">首页</a>
<a href="{% url 'learning_logs:topics' %}">主题</a>
{% block content %}{% endblock content %}
</body>
</html>
```

2】html/index.html

```html
{% extends "html/base.html" %}
{% block content %}
<p>武侠世界欢迎您！</p>
{% endblock content %}
```

3】html/topics.html

```html
{% extends "html/base.html" %}
{% block content %}
<p>图书列表：</p>
<ul>
    {% for topic in topics %}
        <li>
            <a href="{% url 'learning_logs:topic' topic.id %}">{{ topic }}</a>
        </li>
    {% empty %}
        <li>无图书</li>
    {% endfor %}
</ul>
{% endblock content %}
```

3】html/topic.html

```html
{% extends "html/base.html" %}
{% block content %}
<p>图书: {{ topic }}</p>
<p>简介:</p>
<ul>
    {% for entry in entries %}
        <li>
            <p>{{ entry.content}}</p>
            <p>{{ entry.ctime|date:'Y-m-d H:i'}}</p>
        </li>
    {% empty %}
        <li>无简介</li>
    {% endfor %}
</ul>
{% endblock content %}
```



