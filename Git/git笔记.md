GitHub实现快速业务增长的最重要因素之一就是它的商业模式的十分简洁。如果用户想公开托管代码，可以一直免费地使用GitHub。但如果想使用私有存储库或专有的代码托管服务，则需要付费，通过这两种形式，消除了GitHub用免费增值产品蚕食其受众的风险。

GitHub的盈利方式主要有三种：代码托管服务、数据沉淀和云储存服务、企业猎头招聘服务。



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



###### 4.10 关联本地仓库与远程仓库

```shell
$ git remote add origin https://github.com/linwenhai/Notes.git
$ git remote -v
```

`-v`查看当前配置关联哪些远程仓库



###### 4.11 将GitHub上仓库的内容pull到本地仓库

（同步Github代码到本地仓库）

git pull = git fetch + git merge
git pull --rebase = git fetch + git rebase

```shell
$ git pull origin master
```

```shell
$ git pull --rebase origin master
```





###### 4.12 将本地分支推送到Github上分支

（同步本地代码到Github仓库）

git push <远程主机名> <本地分支名>  <远程分支名> 

```shell
$ git push origin master：refs/for/master
```

将本地的master分支推送到远程主机origin上的对应master分支， origin 是远程主机名。



```shell
$ git push origin master
```

如果远程分支被省略，则表示将本地分支推送到与之存在追踪关系的远程分支（通常两者同名），如果该远程分支不存在，则会被新建。





###### 4.12 从远程库克隆到本地

```shell
$ cd d:
$ git clone https://github.com/linwenhai/Notes.git
```





