Mac OS X 10.14上 安装odoo 13.0开发环境
====================================
0.准备
------
假设homebrew已经安装好
没安装的需要先安装，见下面链接
http://brew.sh/

1.安装PostgreSQL
----------------
$ brew tap homebrew/services
$ brew install postgresql
查看安装的版本
$ pg_ctl -V
做为服务启动，停止将start改为stop
brew services start postgresql
不做为服务启动
pg_ctl -D /usr/local/var/postgres start
我这里选择安装为服务

访问缺省数据库
psql postgres
创建新用户和数据库
createuser -P odoodev
ALTER USER odoodev WITH SUPERUSER 
createdb -Oodoodev -Eutf8 odoo11
访问
psql -Uodoodev odoo11
\q 退出
psql的命令这里不再赘述。
postgresql的配置文件在/usr/local/var/postgres/postgresql.conf，修改下面两行
listen_addresses = '*'
unix_socket_directories = '/tmp,/var/pgsql_socket'
brew services stop postgresql
sudo mkdir pgsql_socket
sudo chmod 777 pqsql_socket

2.安装python3
--------------
$ xcode-select —install
$brew install python3
3.安装nodejs和less
nodejs的安装略
sudo npm install -g less
4.下载git源码
-------------
git客户端安装略
git clone https://github.com/odoo/odoo.git
安装Python依赖

$ cd odoo
$ pip3 install -r requirements.txt

5.修改配置文件
------------
运行
./odoo-bin -s
生成配置文件~/.odoorc

cp ~/.odoorc odoo.conf

在odoo.conf中修改下面两行

logfile = /Users/jiangguoqing/odoo/odoo.log
logrotate = True

6.运行
-------

./odoo-bin -c ./odoo.conf

http://localhost:8069/

填写好数据库名称和管理员邮箱密码就可以开始用起了。
7.问题解决
-----------
ValueError: unknown locale: UTF-8
需要编辑~/.bash_profile 加入
两行
export LC_ALL=en_US.UTF-8
export LANG=en_US.UTF-8

If you need to have postgresql@10 first in your PATH run:
  echo 'export PATH="/usr/local/opt/postgresql@10/bin:$PATH"' >> ~/.bash_profile

For compilers to find postgresql@10 you may need to set:
  export LDFLAGS="-L/usr/local/opt/postgresql@10/lib"
  export CPPFLAGS="-I/usr/local/opt/postgresql@10/include"

To have launchd start postgresql@10 now and restart at login:
  brew services start postgresql@10
Or, if you don't want/need a background service you can just run:
  
pg_ctl -D /usr/local/var/postgresql@10 start
pg_ctl -D /usr/local/var/postgresql@10 stop

netstat -nat |grep LISTEN

lsof -i :8069 |grep -i listen | awk '{print $2}' |xargs kill -9

File "/usr/local/python3/lib/python3.7/ssl.py", line 97, in <module>
    import _ssl             # if we can't import it, let the error propagate
ModuleNotFoundError: No module named '_ssl'
问题的原因是虽然mac系统已经装了openssl包及相关的头文件，但是默认python3.7的源码包中指定的ssl头文件目录不包含mac上openssl的路径
备注：mac os 没有openssl-devel这个安装包，内容都已经在openssl中了
[@localhost:openssl]$ brew link openssl --force
Warning: Refusing to link macOS-provided software: openssl
If you need to have openssl first in your PATH run:
  echo 'export PATH="/usr/local/opt/openssl/bin:$PATH"' >> ~/.bash_profile
 
For compilers to find openssl you may need to set:
  export LDFLAGS="-L/usr/local/opt/openssl/lib"
  export CPPFLAGS="-I/usr/local/opt/openssl/include"
