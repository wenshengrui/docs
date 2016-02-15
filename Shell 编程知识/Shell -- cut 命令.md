	Linux Shell脚本编程－－cut命令

cut命令可以从一个文本文件或者文本流中提取文本列。
cut语法
[root@www ~]# cut -d'分隔字符' -f fields <==用于有特定分隔字符
[root@www ~]# cut -c 字符区间            <==用于排列整齐的信息
选项与参数：
-d  ：后面接分隔字符。与 -f 一起使用；
-f  ：依据 -d 的分隔字符将一段信息分割成为数段，用 -f 取出第几段的意思；
-c  ：以字符 (characters) 的单位取出固定字符区间；
 
PATH 变量如下
[root@www ~]# echo $PATH
/bin:/usr/bin:/sbin:/usr/sbin:/usr/local/bin:/usr/X11R6/bin:/usr/games
# 1 | 2       | 3   | 4       | 5            | 6            | 7
 
将 PATH 变量取出，我要找出第五个路径。
#echo $PATH | cut -d ':' -f 5
/usr/local/bin
 
将 PATH 变量取出，我要找出第三和第五个路径。
#echo $PATH | cut -d ':' -f 3,5
/sbin:/usr/local/bin
 
将 PATH 变量取出，我要找出第三到最后一个路径。
echo $PATH | cut -d ':' -f 3-
/sbin:/usr/sbin:/usr/local/bin:/usr/X11R6/bin:/usr/games
 
将 PATH 变量取出，我要找出第一到第三个路径。
#echo $PATH | cut -d ':' -f 1-3
/bin:/usr/bin:/sbin:
 
 
将 PATH 变量取出，我要找出第一到第三，还有第五个路径。
echo $PATH | cut -d ':' -f 1-3,5
/bin:/usr/bin:/sbin:/usr/local/bin
 
实用例子:只显示/etc/passwd的用户和shell
#cat /etc/passwd | cut -d ':' -f 1,7 
root:/bin/bash
daemon:/bin/sh
bin:/bin/sh