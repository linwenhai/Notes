## HTML

HTML是 HyperText Mark-up Language 的首字母简写，意思是超文本标记语言。



### 1 基本结构

```html
<!DOCTYPE html>
<html lang="en">
    <head>            
        <meta charset="UTF-8">
        <title>网页标题</title>
    </head>
    <body>
          网页显示内容
    </body>
</html>
```



html注释：

```html
<!-- 这是一段注释  -->
```



### 2 标题标签

```html
<h1>这是1级标题</h1>
<h2>这是2级标题</h2>
<h3>这是3级标题</h3>
<h4>这是4级标题</h4>
<h5>这是5级标题</h5>
<h6>这是6级标题</h6>
```



### 3 段落标签

标签定义一个文本段落，一个段落含有默认的上下间距，段落之间会用这种默认间距隔开。

```
<p></p>
```



### 4 换行标签

代码中成段的文字，直接在代码中回车换行，在渲染成网页时候不认这种换行，如换行使用换行标签。

```
<br />
```



### 5 字符实体

```
空格：&nbsp;
>：&gt;
<：&lt;
```



### 6 语义化的标签

h1：标签是表示标题

p：标签是表示段落

ul、li：标签是表示列表

a：标签表示链接

dl、dt、dd：表示定义列表



### 7 图像标签

```html
<img src="images/pic.jpg" alt="产品图片" />
```

- src属性：定义图片的引用地址
- alt属性：定义图片加载失败时显示的文字

- “ ./ ” ：表示当前文件所在目录下
- “ ../ ” ：表示当前文件所在目录下的上一级目录



### 8 链接标签

```
<a></a>	标签可以在网页上定义一个链接地址
```

```html
<a href="#"></a> <!--  # 表示链接到页面顶部   -->
<a href="http://www.itcast.cn/" title="跳转的传智播客网站">传智播客</a>
<a href="2.html" target="_blank">测试页面</a>
```

- href属性：定义跳转的地址
- title属性：定义鼠标悬停时弹出的提示文字框
- target属性：定义链接窗口打开的位置
  - target="_self" 缺省值，新页面替换原来的页面，在原来位置打开
  - target="_blank" 新页面会在新开的一个浏览器窗口打开



### 9 列表标签

```html
<!-- 有序列表，每条项目上会按1、2、3编号  -->
<ol>
    <li>列表文字一</li>
    <li>列表文字二</li>
    <li>列表文字三</li>
</ol>
```



```html
<!-- 无序列表  -->
<ul>
    <li><a href="#">新闻标题一</a></li>
    <li><a href="#">新闻标题二</a></li>
    <li><a href="#">新闻标题三</a></li>
</ul>
```



### 10 定义列表

```html
<h3>前端三大块</h3>
<dl>
    <dt>html</dt>
    <dd>负责页面的结构</dd>

    <dt>css</dt>
    <dd>负责页面的表现</dd>

    <dt>javascript</dt>
    <dd>负责页面的行为</dd>

</dl>
```

```
<dl>标签表示列表的整体
<dt>标签定义术语的题目
<dd>标签是术语的解释
```



### 11 表单

```
<form></from>	：定义整体的表单区域
```

- action属性定义表单数据提交地址
- method属性定义表单提交的方式，一般有“get”方式和“post”方式

```
<label></label>	：为表单元素定义文字标注
```

```
<input />	：定义通用的表单元素
```

- value属性 定义表单元素的值
- name属性 定义表单元素的名称，此名称是提交数据时的键名
- type属性

>type="text" 定义单行文本输入框
>
>type="password" 定义密码输入框
>
>type="radio" 定义单选框
>
>type="checkbox" 定义复选框
>
>type="file" 定义上传文件
>
>type="submit" 定义提交按钮
>
>type="reset" 定义重置按钮
>
>type="button" 定义一个普通按钮
>
>type="image" 定义图片作为提交按钮，用src属性定义图片地址
>
>type="hidden" 定义一个隐藏的表单域，用来存储值

```
<textarea></textarea>	：定义多行文本输入框
```

```
<select></select>	：定义下拉表单元素
<option></option>	：与<select>标签配合，定义下拉表单元素中的选项
```



```html
<!-- 注册表单实例  -->
<form action="http://www..." method="get">
<p>
<label>姓名：</label><input type="text" name="username" />
</p>
<p>
<label>密码：</label><input type="password" name="password" />
</p>
<p>
<label>性别：</label>
<input type="radio" name="gender" value="0" /> 男
<input type="radio" name="gender" value="1" /> 女
</p>
<p>
<label>爱好：</label>
<input type="checkbox" name="like" value="sing" /> 唱歌
<input type="checkbox" name="like" value="run" /> 跑步
<input type="checkbox" name="like" value="swiming" /> 游泳
</p>
<p>
<label>照片：</label>
<input type="file" name="person_pic">
</p>
<p>
<label>个人描述：</label>
<textarea name="about"></textarea>
</p>
<p>
<label>籍贯：</label>
<select name="site">
    <option value="0">北京</option>
    <option value="1">上海</option>
    <option value="2">广州</option>
    <option value="3">深圳</option>
</select>
</p>
<p>
<input type="submit" name="" value="提交">
<!-- input类型为submit定义提交按钮  
     还可以用图片控件代替submit按钮提交，一般会导致提交两次，不建议使用。如：
     <input type="image" src="xxx.gif">
-->
<input type="reset" name="" value="重置">
</p>
</form>
```



### 12 表格













