## GIT命令

### 1 git配置

```shell
# 查看现在的git环境详细配置
git config -l
```



```shell
# 设置用户名与邮箱
git config --global user.name "[名称]" 
git config --global user.email "[邮箱]"
```



### 2 克隆远程仓库至本地

```shell
git clone [克隆连接]
# 如：
git clone https://github.com/linwenhai/Notes.git
```



### 3 远程仓库同步至本地仓库

```shell
git pull origin master
```



### 4 提交修改至远程仓库

```shell
git add .
git commit -m "2020-08-29"
git push origin master
```



### 5 Github国内镜像

```bash
git clone https://github.com/linwenhai/Notes.git
# 改为：
git clone https://github.com.cnpmjs.org/linwenhai/Notes.git
或
git clone https://git.sdut.me/linwenhai/Notes.git
```

更多国内镜像站参考：https://doc.sdut.me/mirror.html



### 6 阿里云code

网址：https://code.aliyun.com/

```bash
git clone https://code.aliyun.com/linwenhai/Notes.git
```

