	Linux Shell脚本编程－－wc 命令

wc 统计文件里面有多少单词，多少行，多少字符。
wc 语法
[root@www ~]# wc [-lwm]
选项与参数：
-l  ：仅列出行；
-w  ：仅列出多少字(英文单字)；
-m  ：多少字符；
 
默认使用wc统计/etc/passwd
#wc /etc/passwd
40   45 1719 /etc/passwd
40是行数，45是单词数，1719是字节数
 
wc的命令比较简单使用，每个参数使用如下：
#wc -l /etc/passwd   #统计行数，在对记录数时，很常用
40 /etc/passwd       #表示系统有40个账户

#wc -w /etc/passwd  #统计单词出现次数
45 /etc/passwd

#wc -m /etc/passwd  #统计文件的字节数
1719