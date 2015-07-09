		Centos6.5 上 lnmp + SVN 搭建

1. 下载svn安装包
[root@wiki tools] wget http://subversion.tigris.org/downloads/subversion-1.6.1.tar.gz
[root@wiki tools] wget http://subversion.tigris.org/downloads/subversion-deps-1.6.1.tar.gz 

2.安装svn
[root@wiki tools] tar -zxvf subversion-1.6.1.tar.gz
[root@wiki tools] tar -zxvf subversion-deps-1.6.1.tar.gz
[root@wiki tools] cd subversion-1.6.1 
[root@wiki tools] ./configure --prefix=/usr/local/svn/
[root@wiki tools] make && make install 

把svn相关的命令添加到环境变量中
[root@wiki tools] echo "export PATH=$PATH:/usr/local/svn/bin/" >> /etc/profile 

(source命令通常用于重新执行刚修改的初始化文件，使之立即生效，而不必注销并重新登录)
[root@wiki tools] source /etc/profile
[root@wiki tools] ln /usr/local/svn/bin/svn /sbin/svn

3. 创建svn版本库
建立 SVN 的根目录,根目录是svn启动的时候指定的
[root@wiki tools] mkdir -p /opt/svn/

建立一个测试仓库
[root@wiki tools] mkdir -p /opt/svn/svntest/
[root@wiki tools] svnadmin create /opt/svn/svntest/

4. 创建svn配置文件
[root@wiki tools] mkdir -p /opt/svnpasswd/
[root@wiki tools] cp /opt/svn/svntest/conf/passwd /opt/svnpasswd/
[root@wiki tools] cp /opt/svn/svntest/conf/authz /opt/svnpasswd/
[root@wiki tools] cp /opt/svn/svntest/conf/svnserve.conf /opt/svnpasswd/

5. 修改svn配置文件(将下面语句前的#去掉，注意与行首不要有空格)
[root@wiki tools] vim /opt/svn/svntest/conf/svnserve.conf
#anon-access	= read   修改为 anon-access = read
#auth-access	= write  修改为 auth-access = write
#password-db	= passwd 修改为 password-db = /opt/svnpasswd/passwd (存放密码的文件为当前目录的passwd文件) 
#authz-db	= authz	 修改为 authz-db    = /opt/svnpasswd/authz  (存放用户的文件为当前目录的authz文件) 

添加用户和密码
格式为：  
[users]
<用户1> = <密码1> 
<用户2> = <密码2>

[root@wiki tools] vim /opt/svnpasswd/passwd
用户 = 密码
svntest = test123

3.2 添加用户权限   
格式： 
版本库目录格式： 
[<版本库>:/项目/目录] 
@<用户组名> = <权限> 
<用户名> = <权限>  
[root@wiki tools] vim /opt/svnpasswd/authz
[svntest:/]
svntest=rw

解释：/表示根目录，根目录是svnserve启动时指定的 权限由：r ,w ,和空，空表示没有权限 
如：[svntest :/ a]  ->表示对svntest下的a目录设置权限

6. 启动svn服务
[root@wiki tools] svnserve -d --listen-port 9999 -r /opt/svn/
[root@wiki tools] ps -ef | grep svnserve

参数分析其中： 
-d表示以daemon方式（后台运行）运行 
--listen-port 9999表示使用9999端口，可以换成你需要的端口。但注意，使用1024以下的端口需要root权限 
-r /opt/svn/svndata指定根目录是/opt/svn/svntest 

检查: 
ps –ef | grep svnserve 
如果显示如下，即为启动成功： 
root     15908     1  0 21:50 ?        00:00:00 svnserve -d --listen-port 9999 -r  /opt/svn   


7. 项目与svn同步
[root@wiki tools] cd /data/web/
[root@wiki tools] svn co file:///opt/svn/svntest/
[root@wiki tools] touch /opt/svn/svntest/hooks/post-commit
[root@wiki tools] chmod 700 /opt/svn/svntest/hooks/post-commit
[root@wiki tools] vim /opt/svn/svntest/hooks/post-commit
#!/bin/bash
export LC_ALL=en_US.UTF-8
/sbin/svn up /data/web/svntest

[root@wiki tools] ll /data/web/svntest/
