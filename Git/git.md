##### 1 安装GIT

Git-2.21.0-64-bit.exe



##### 2 登陆Github

```shell
$ git config --global user.name "Your Name"
$ git config --global user.email "email@example.com"
```



##### 3 创建repository

```shell
$ cd d:
$ mkdir Notes
$ cd 
$ pwd
/d/Notes
```

如果你使用Windows系统，为了避免遇到各种莫名其妙的问题，请确保目录名（包括父目录）不包含中文。



##### 4 Git命令

###### 4.1 初始化一个Git仓库

```shell
$ git init
Initialized empty Git repository in D:/Notes/.git/
```



###### 4.2 添加文件到GIT仓库暂存区Stage

```shell
$ git add git.md
```



###### 4.3 把GIT库暂存区的所有内容提交到当前分支master

```shell
$ git commit -m "20190410 14:33"
```

`-m`后面输入的是本次提交的说明



###### 4.4 查看仓库当前的状态

```shell
$ git status
```



###### 4.5 查看修改内容

```shell
$ git diff git.md
```



###### 4.6 查看版本信息

```shell
$ git log --pretty=oneline
```



###### 4.7 回退版本

```shell
$ git reset --hard HEAD^
```

`HEAD`表示当前版本，上一个版本就是`HEAD^`，上上一个版本就是`HEAD^^`



###### 4.8 查看命令历史

```shell
$ git reflog
```



###### 4.9 删除文件

```shell
$ rm test.txt
$ git status
$ git rm test.txt
$ git commit -m "remove test.txt"
```



###### 4.10 把本地仓库与远程仓库关联

```shell
$ git remote add origin https://github.com/linwenhai/Notes.git
$ git remote -v
```

`-v`查看当前配置有哪些远程仓库



###### 4.11 上传项目到Github

```shell
$ git push -u origin master
```

第一次推送`master`分支时，加上了`-u`参数

```shell
$ git push origin master
```



如本地库和远程库不一致，需把远程库中的更新合并到本地库中，再上传

```shell
$ git pull --rebase origin master
```



4.14 从远程库克隆到本地



