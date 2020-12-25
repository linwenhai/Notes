## Sqlite3升级到3.27.2

Django2.2以上需使用高版本SQLite

异常错误：SQLite 3.8.3 or later is required (found 3.7.17)

```bash
# 检测OS版本
sqlite3 --version
-------OS自带版本---------------
3.7.17 2013-05-20 00:56:22 118a3b35693b134d56ebd780123b7fd6f1497668
--------------------------------
```



```bash
# 安装sqlite 3.27.2
wget https://www.sqlite.org/2019/sqlite-autoconf-3270200.tar.gz
tar -zxvf sqlite-autoconf-3270200.tar.gz
cd sqlite-autoconf-3270200
./configure --prefix=/usr/local
make && make install
 
# 检测新版本
/usr/local/bin/sqlite3 --version
--------------------------------------------
3.27.2 2019-02-25 16:06:06 bd49a8271d650fa89e446b42e513b595a717b9212c91dd384aab871fc1d0f6d7
--------------------------------------------
```



```bash
# 替换系统低版本sqlite3
mv /usr/bin/sqlite3  /usr/bin/sqlite3_old
ln -s /usr/local/bin/sqlite3   /usr/bin/sqlite3
echo "/usr/local/lib" > /etc/ld.so.conf.d/sqlite3.conf
ldconfig
```



```bash
# 验证
sqlite3 --version
-----------------------------------------
3.27.2 2019-02-25 16:06:06 bd49a8271d650fa89e446b42e513b595a717b9212c91dd384aab871fc1d0f6d7
-----------------------------------------
```









