## Gitbash



```bash
# 查看现在的git环境详细配置
git config -l

# 设置用户名与邮箱
git config --global user.name "[名称]" 
git config --global user.email "[邮箱]"
```



```bash
# 克隆远程仓库至本地
git clone https://github.com/linwenhai/Notes.git

# Github国内镜像
git clone https://github.com.cnpmjs.org/linwenhai/Notes.git
```



```bash
# 远程仓库同步至本地仓库
git pull origin master

# 提交修改至远程仓库
git add .
git commit -m "2021-01-01"
git push origin master
```

