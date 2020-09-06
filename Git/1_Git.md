

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



### 5 加速国内访问Github

**1】获取GitHub官方CDN地址**

打开https://www.ipaddress.com/

查询以下三个链接的DNS解析地址

- github.com
- assets-cdn.github.com
- github.global.ssl.fastly.net

**2】修改系统Hosts文件**

打开系统hosts文件(需管理员权限)。
路径：C:\Windows\System32\drivers\etc

**3】刷新系统DNS缓存**

Windows+X 打开系统命令行（管理员身份）或powershell

运行 ipconfig /flushdns 手动刷新系统DNS缓存。