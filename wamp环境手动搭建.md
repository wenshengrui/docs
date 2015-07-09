<<<<<<< HEAD
wamp环境手动搭建 apache2.4+php5.5+mysql5.6 

1. 下载相应的软件
http://download.microsoft.com/download/1/6/B/16B06F60-3B20-4FF2-B699-5E9B7962F9AE/VSU_4/vcredist_x64.exe
http://www.apachelounge.com/download/VC11/binaries/httpd-2.4.12-win64-VC11.zip
http://windows.php.net/downloads/releases/php-5.5.25-Win32-VC11-x64.zip
http://cdn.mysql.com/Downloads/MySQL-5.6/mysql-5.6.24-winx64.zip

2. 安装 Apache2.4
(1)首先安装 VC11 vcredist_x64.exe，若干个下一步就OK
(2)将 httpd-2.4.12-win64-VC11.zip 解压到 D:\WampServer\Apache24 这个目录下；
(3)修改配置文件 D:\WampServer\Apache24\conf\httpd.conf 
修改 37行 ServerRoot "" => ServerRoot "D:\WampServer\Apache24"（这里输入的是你解压apache安装包的位置）
修改 217行 #ServerName www.example.com:80 => ServerName www.example.com:80 (去掉前面的#)
修改 241行 DocumentRoot "c:/Apache24/htdocs" => DocumentRoot "d:\www"
修改 242行 <Directory "c:/Apache24/htdocs"> => <Directory "d:\www">
修改 275行 DirectoryIndex index.html => DirectoryIndex index.html index.php index.htm
修改 358行 ScriptAlias /cgi-bin/ "c:/Apache24/cgi-bin/"  => ScriptAlias /cgi-bin/ " D:\WampServer\Apache24\cgi-bin\ "
修改 374行 <Directory "c:/Apache24/cgi-bin"> => <Directory "D:\WampServer\Apache24\cgi-bin">

a.让apache支持php
添加 117行后 LoadModule php5_module "D:\WampServer\Php5.5\php5apache2_4.dll" （请确认D:\WampServer\Php5.5中有php5apache2_4.dll）
添加 最后一行 AddType application/x-httpd-php .php .html .htm

b.告诉apache php.ini的位置
=======
1. 下载相应的软件

http://download.microsoft.com/download/1/6/B/16B06F60-3B20-4FF2-B699-5E9B7962F9AE/VSU_4/vcredist_x64.exe

http://www.apachelounge.com/download/VC11/binaries/httpd-2.4.12-win64-VC11.zip

http://windows.php.net/downloads/releases/php-5.5.25-Win32-VC11-x64.zip

http://cdn.mysql.com/Downloads/MySQL-5.6/mysql-5.6.24-winx64.zip


2. 安装 Apache2.4

(1)首先安装 VC11 vcredist_x64.exe，若干个下一步就OK

(2)将 httpd-2.4.12-win64-VC11.zip 解压到 D:\WampServer\Apache24 这个目录下；

(3)修改配置文件 D:\WampServer\Apache24\conf\httpd.conf 

修改 37行 ServerRoot "" 改为 ServerRoot "D:\WampServer\Apache24"（这里输入的是你解压apache安装包的位置）

修改 217行 #ServerName www.example.com:80 改为 ServerName www.example.com:80 (去掉前面的#)

修改 241行 DocumentRoot "c:/Apache24/htdocs" 改为 DocumentRoot "d:\www"

修改 242行 Directory "c:/Apache24/htdocs" 改为 Directory "d:\www"

修改 275行 DirectoryIndex index.html 改为 DirectoryIndex index.html index.php index.htm

修改 358行 ScriptAlias /cgi-bin/ "c:/Apache24/cgi-bin/" 改为 ScriptAlias /cgi-bin/ " D:\WampServer\Apache24\cgi-bin\ "

修改 374行 Directory "c:/Apache24/cgi-bin" 改为 Directory "D:\WampServer\Apache24\cgi-bin"

a.让apache支持php

添加 117行后 LoadModule php5_module "D:\WampServer\Php5.5\php5apache2_4.dll"
（请确认D:\WampServer\Php5.5中有php5apache2_4.dll）

添加 最后一行 AddType application/x-httpd-php .php .html .htm

b.告诉apache php.ini的位置

>>>>>>> ea93dcd8ef2bb27bf22cc3b1e28b62172ea71ecd
添加 PHPIniDir "D:\WampServer\Php5.5"

c.保存 D:\WampServer\Apache24\conf\httpd.conf

<<<<<<< HEAD
(4)安装 Apache2.4
=======

(4)安装 Apache2.4

>>>>>>> ea93dcd8ef2bb27bf22cc3b1e28b62172ea71ecd
在cmd中执行：D:\WampServer\Apache24\bin\httpd -k install


3. 安装 Php5.5
<<<<<<< HEAD
(1)将 php-5.5.25-Win32-VC11-x64.zip 解压到 D:\WampServer\Php5.5 这个目录下；
(2)将 D:\WampServer\Php5.5\php.ini-production 复制一份，并重命名为php.ini；
(3)修改配置文件 D:\WampServer\Php5.5\php.ini
修改 721行 将 ;extension_dir = "ext"		=> extension_dir = "ext" （去掉extension前面的分号）
修改 873行 将 ;extension=php_mbstring.dll	=> extension=php_mbstring.dll（去掉extension前面的分号,这是php多字节字符串扩展） 
修改 875行 将 ;extension=php_mysql.dll		=> extension=php_mysql.dll（去掉extension前面的分号）
修改 876行 将 ;extension=php_mysqli.dll		=> extension=php_mysqli.dll（去掉extension前面的分号）
=======

(1)将 php-5.5.25-Win32-VC11-x64.zip 解压到 D:\WampServer\Php5.5 这个目录下；

(2)将 D:\WampServer\Php5.5\php.ini-production 复制一份，并重命名为php.ini；

(3)修改配置文件 D:\WampServer\Php5.5\php.ini

修改 721行 将 ;extension_dir = "ext"	去掉extension前面的分号 extension_dir = "ext" 

修改 873行 将 ;extension=php_mbstring.dll	去掉extension前面的分号 extension=php_mbstring.dll 

修改 875行 将 ;extension=php_mysql.dll		去掉extension前面的分号 extension=php_mysql.dll

修改 876行 将 ;extension=php_mysqli.dll		去掉extension前面的分号 extension=php_mysqli.dll

>>>>>>> ea93dcd8ef2bb27bf22cc3b1e28b62172ea71ecd
(4)启动服务
net start apache2.2

(5)测试Apache是否安装成功
<<<<<<< HEAD
http://localhost/phpinfo.php

4. 安装 Mysql5.6 
(1)将 mysql-5.6.24-winx64.zip 解压到 D:\WampServer\Mysql5.6 这个目录下；
(2)把根目录下的my-default.ini重命名为my.ini，编辑文件。
(3)修改配置文件 my.ini

[mysqld]
#新版不支持在my.ini中直接设置字符集为utf8。解决方法是在default-character-set前面加上loose-。
loose-default-character-set = utf8

#加loose-后MySQL启动是不再报错了，但是在插入数据时依然出现了乱码问题。解决方法是加入character-set-server。
character-set-server = utf8

#如果是服务器用的话，建议设大点。
=======

http://localhost/phpinfo.php

4. 安装 Mysql5.6 

(1)将 mysql-5.6.24-winx64.zip 解压到 D:\WampServer\Mysql5.6 这个目录下；

(2)把根目录下的my-default.ini重命名为my.ini，编辑文件。

(3)修改配置文件 my.ini

[mysqld]

#新版不支持在my.ini中直接设置字符集为utf8。解决方法是在default-character-set前面加上loose-。

loose-default-character-set = utf8


#加loose-后MySQL启动是不再报错了，但是在插入数据时依然出现了乱码问题。解决方法是加入character-set-server。

character-set-server = utf8


#如果是服务器用的话，建议设大点。

>>>>>>> ea93dcd8ef2bb27bf22cc3b1e28b62172ea71ecd
innodb_buffer_pool_size = 128M

#基路径
basedir = basedir = D:\WampServer\Mysql5.6

#数据路径
datadir = D:\WampServer\Mysql5.6\data

#日志路径
log_bin = D:\WampServer\Mysql5.6\log

#如果不加这行，默认是监听127.0.0.0，加了后是监听局域网端口和外网端口。
<<<<<<< HEAD
=======

>>>>>>> ea93dcd8ef2bb27bf22cc3b1e28b62172ea71ecd
bind-address = 0.0.0.0

#监听端口
port = 3306

[client]
<<<<<<< HEAD
loose-default-character-set = utf8

[WinMySQLadmin]
Server = D:\WampServer\Mysql5.6/bin/mysqld.exe

(4)安装并启动服务
#安装服务
mysqld -install

#卸载服务
mysqld -remove

#启动服务
net start mysql

#停止服务
net stop mysql

(5).进入MySQL\
命令启动 cmd
D:\WampServer\Mysql5.6/bin/mysql -u root -p

(6).设置密码
mysql> update mysql.user set password=PASSWORD('123456') where user='root'
=======

loose-default-character-set = utf8

[WinMySQLadmin]

Server = D:\WampServer\Mysql5.6/bin/mysqld.exe

(4)安装并启动服务

#安装服务 mysqld -install

#卸载服务 mysqld -remove

#启动服务 net start mysql

#停止服务 net stop mysql

(5).进入MySQL\

命令启动 cmd

D:\WampServer\Mysql5.6/bin/mysql -u root -p

(6).设置密码

mysql> update mysql.user set password=PASSWORD('123456') where user='root'

>>>>>>> ea93dcd8ef2bb27bf22cc3b1e28b62172ea71ecd
mysql> flush privileges

(7).如果要监听外网，除了my.ini要设置bind-address以外，还要设置权限。

mysql> grant all privileges on *.* to root@"%" identified by 'mypassword' with grant option;
<<<<<<< HEAD
mysql> flush privileges;
其中%表示任意地址可登入，也可以指定具体IP，例如"192.168.1.102"。

5.默认启动
将一下信息添加到环境变量中
path中输入：D:\WampServer\Php5.5;D:\WampServer\Php5.5\ext;D:\WampServer\Apache24\bin;D:\WampServer\Mysql5.6\bin;
=======

mysql> flush privileges;

其中%表示任意地址可登入，也可以指定具体IP，例如"192.168.1.102"。

5.默认启动

将一下信息添加到环境变量中

path中输入：D:\WampServer\Php5.5;D:\WampServer\Php5.5\ext;D:\WampServer\Apache24\bin;D:\WampServer\Mysql5.6\bin;
>>>>>>> ea93dcd8ef2bb27bf22cc3b1e28b62172ea71ecd
