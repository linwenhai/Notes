## AliyunCode

### 1 配置user name和email

```bash
git config --global user.name "your name"
git config --global user.email "your email"
```

```bash
git config --global user.name
git config --global user.email
git config --global --list
```



### 2 克隆项目

```bash
git clone https://code.aliyun.com/linwenhai/Notes.git
```



### 3 下载项目的最新更改

```bash
git pull origin master -u
```



### 4 浏览更改

```bash
git status
```



### 5 提交更改到服务器

```bash
git add .
git commit -m "2020-09-16"
git push origin master
```

