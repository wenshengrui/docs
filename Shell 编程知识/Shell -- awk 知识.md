awk 命令总结
一、基础知识
1. AWK 是一个强大的文本分析或数据处理工具，在对文本文件的处理以及生成报表，awk是无可替代的。awk认为文本文件都是结构化的，它将每一个输入行定义为一个记录，行中的每个字符串定义为一个域(段)，域和域之间使用分割符分割。

2. AWK 的功能：流控制、数学运算、进程控制、内置的变量和函数、循环和判断

3. AWK 的工作原理：
awk 会把每行进行一个拆分，用相应的命令对拆分出来的“段”进行处理。
（1）行工作模式，读入文件的每一行，会把一行的内容，存到$0里
（2）使用内置的变量FS(段的分隔符，默认用的是空白字符)，分割这一行，把分割出来的每个段存到相应的变量$(1-100)
（3）输出的时候按照内置变量OFS(out FS)，输出
（4）读入下一行继续操作

简单实例
[root@tx3 ~]# echo "this is a book" > awk.txt
[root@tx3 ~]# awk '{print $2,$1,$3,$4}' awk.txt
is this a book

4. AWK 命令的基本语法
awk pattern { actions } 表达的含义：当某个文本符合pattern指定的匹配规则时，执行actions所执行的操作。
pattern 匹配模式
actions 要执行的操作

注：pattern 与 actions 是可选参数，但是两者必须保证至少有一个。actions 前面的左大括号需与pattern位于同一行上。
	如果省略pattern，则表示对所有的文本行执行 actions 的所有操作；如果省略actions，则表示将匹配成功的行输出到屏幕上。

	AWK 命令的语法结构隐含一个条件结构，即如果符合匹配规则，则执行后面的操作。
	AWK 命令的操作由一个或多个命令、函数或者表达式组成，它们之间由转换符或者分号隔开。并且位于大括号内。

5. 执行AWK程序的几种方式：命令行，awk脚本，以及可执行的文件


6. AWK 常用内置变量表：
$0             当前记录（作为单个变量）  
$1~$n          当前记录的第n个字段，字段间由FS分隔  
FS             输入字段分隔符 默认是空格  
NF             当前记录中的字段个数，就是有多少列  
NR             已经读出的记录数，就是行号，从1开始  
RS             输入的记录他隔符默 认为换行符  
OFS            输出字段分隔符 默认也是空格  
ORS            输出的记录分隔符，默认为换行符  
ARGC           命令行参数个数  
ARGV           命令行参数数组  
FILENAME       当前输入文件的名字  
IGNORECASE     如果为真，则进行忽略大小写的匹配  
ARGIND         当前被处理文件的ARGV标志符  
CONVFMT        数字转换格式 %.6g  
ENVIRON        UNIX环境变量  
ERRNO          UNIX系统错误消息  
FIELDWIDTHS    输入字段宽度的空白分隔字符串  
FNR            当前记录数  
OFMT           数字的输出格式 %.6g  
RSTART         被匹配函数匹配的字符串首  
RLENGTH        被匹配函数匹配的字符串长度

二．print的简单使用  
打印整行: $0
[root@tx3 ~]# cp /etc/passwd p1
[root@tx3 ~]# awk '{print $0}' p1

打印每行的最后一个字段: $NF
[root@tx3 ~]# awk -F : '{print $NF}' p1

打印第三个字段: $3
[root@tx3 ~]# awk -F : '{print $3}' p1

打印第一行NR==1
[root@tx3 ~]# awk 'NR==1{print $0}' p1
root:x:0:0:root:/root:/bin/bash

打印最后一行
[root@tx3 ~]# awk 'END{print $0}' p1
www:x:501:501::/home/www:/bin/bash

打印第一行最后一个字段
[root@tx3 ~]# awk -F: 'NR==1{print $NF}' p1
/bin/bash

打印最后一行最后一个字段
[root@tx3 ~]#awk -F: 'END{print $NF}' p1

打印每行的倒数第二个字段，并在其后打印你好
[root@tx3 ~]# awk -F: '{print $(NF-1),"nihao"}' p1
/root nihao
/bin nihao
/sbin nihao


例：打印行号
[root@tx3 ~]# awk '{print NR,$0}' p1
1 root:x:0:0:root:/root:/bin/bash
2 bin:x:1:1:bin:/bin:/sbin/nologin
3 daemon:x:2:2:daemon:/sbin:/sbin/nologin

例：打印当前系统环境变量的某个特定值
[root@tx3 ~]# awk 'BEGIN{print ENVIRON["PATH"];}'
/usr/kerberos/sbin:/usr/kerberos/bin:/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin:/root/bin

例： 用:分割，删除第2个字段
[root@tx3 ~]# awk 'BEGIN{FS=":";OFS=":"}{print $1,$3,$4,$5,$6,$7}' p1
root:0:0:root:/root:/bin/bash
bin:1:1:bin:/bin:/sbin/nologin
daemon:2:2:daemon:/sbin:/sbin/nologin

三．printf的使用
print format 生成报表
%d        十进制有符号整数  
%u        十进制无符号整数  
%f        浮点数  
%s        字符串  
%c        显示字符的ASCII码  
%p        指针的值  
%e        科学技术法显示数值  
%x        %X 无符号以十六进制表示的整数  
%o        无符号以八进制表示的整数  
%g        %G 以科学计数法或浮点数的格式显示数值  
%%        显示其自身

修饰符：  
-:  左对齐     
+:  显示数值符号  
N： 显示

-F 指定段的分隔符
例：（1）生成报表 

例：（2）小数问题
对小数取保留位的时候，四舍五入 
对小数取整，不进行四舍五入
[root@tx3 ~]# cat awk.1
23.3456 11.234 45.67
[root@tx3 ~]# awk '{printf "%.2f\t%.2f\t%.2f\n",$1,$2,$3}' awk.1
23.3511.2345.67

四．awk的使用

（1）正则表达式
\(\)   \{\} 不支持
. * ^ $ ? + [] | \< \> ()  可以直接使用

例[root@tx3 ~]# awk '/^$/{print "this is an empty line"}' /etc/inittab
this is an empty line
this is an empty line
this is an empty line
this is an empty line
this is an empty line
this is an empty line
this is an empty line
this is an empty line
this is an empty line

例[root@tx3 ~]# awk -F: '/^root/{print $1,$NF}' /etc/passwd
root /bin/bash

例[root@tx3 ~]# awk -F: '!/^root/{print $1,$NF}' /etc/passwd|head -3  
bin /sbin/nologin
daemon /sbin/nologin
adm /sbin/nologin 


（2）关系运算符
> < == != >= <=
~（匹配） !~（不匹配）
例[root@tx3 ~]# cp /etc/passwd p1
[root@tx3 ~]# awk -F: '$3 == 0 {print $1}' p1
Root

例[root@tx3 ~]# awk -F: '$3 != 0{ print $1}' p1 | head -2
bin
Daemon

例[root@tx3 ~]# awk -F: '$3 < 2 {print $1}' p1
root
bin

（3）逻辑运算符
&& || !
与 或 非
例[root@tx3 ~]# awk -F: '$3 > 0 && $3 < 10 {print $1, $3}' p1 |head -2
bin 1
daemon 2

例[root@tx3 ~]#  awk -F: '$3 > 10 || $3 < 5 {print $1,$3}' p1 |head -6
root 0
bin 1
daemon 2
adm 3
lp 4
operator 11

（4）算数运算符
+ - * / %（取模(余数)） ^（幂运算）

例：输出名字，总成绩，平均成绩
[root@tx3 ~]# cat cj
tx 90 86 86
tx1 89 78 85
tx2 79 80 85   

[root@tx3 ~]#  awk '{print $1,$2+$3+$4,($2+$3+$4)/3}' cj
tx 262 87.3333
tx1 252 84
tx2 244 81.3333

[root@tx3 ~]# awk '{printf"%-5s %3d %.2f\n",$1,$2+$3+$4,($2+$3+$4)/3}' cj
tx    262 87.33
tx1   252 84.00
tx2   244 81.33

（5）BEGIN  END
BEGIN{ 动作;动作;... }  在处理文件之前，要执行的动作；只执行一次
END{ 动作;动作;... }    在处理完文件之后，要执行的动作；只执行一次
BEGIN ：可以给文件添加标题、定义变量、定义文件的分隔符
END：汇总的操作
getline可以从管道和标准输入读取输入，然后传递给变量。

例：
[root@tx3 ~]# awk 'BEGIN{"date"| getline a}{print}END{print a}' cj
tx 90 86 86
tx1 89 78 85
tx2 79 80 85  
Thu Feb  7 12:39:25 CST 2013

五．awk里的流控制和循环
（1）简单的条件判断
语法：(表达式 ? 值1 : 值2) 如果表达式成立，输出值1；否则输出值2
[root@tx3 ~]# cat num
2 8 9
8 4 6
3 5 7
[root@tx3 ~]# awk '{print ( $1 > $2 ? $1 : $2)}' num
8
8
5

（2）if判断
语法：
{ 
if (表达式
{
                动作1;动作2;...
}
}
   如果表达式成立，那么执行动作。
[root@tx3 ~]# awk '{if ($2>=80 && $2 <=100) {print $1,"great"} else {print $1, "good"}}' cj
tx great
tx1 great
tx2 good
（2）多支判断

{
if (表达式)
{ 动作1;动作2;...}
else if (表达式)
{ 动作1;动作2;...}
else if (表达式)
{ 动作1;动作2;...}
......
else
{ 动作1;动作2;...}
}

[root@tx3 ~]# cat cj
tx 90 86 86
tx1 89 78 85
tx2 79 80 85  
tx3 80 70 60
tx4 75 85 65
tx5 78 62 80

判断的标准：
90-100 A
80-89  B
70-79  C
60-69  D
0-59   E
[root@tx3 ~]# awk '{ if ($2 >= 90 && $2 <= 100) {print $1,"A"} else if ($2 >= 80 && $2 < 90) {print $1,"B"} else if ($2 >= 70 && $2 < 80) {print $1,"C"} else if ($2 >= 60 && $2 < 70) {print $1,"D"} else {print $1,"E"} }' cj
tx A
tx1 B
tx2 C
tx3 B
tx4 C
tx5 C
（3）循环while

语法：'var=初值;while (表达式){动作1;...更新变量的动作;}'
例：
[root@tx3 ~]# awk -F: '{i=1; while (i<=NF) {print $i;i++}}' p1 | head -7
root
x
0
0
root
/root
/bin/bash

例. 方法一
[root@tx3 ~]# awk -F: '{i=NF; while (i>=2) {printf $i ":";i--};print $1}' p1
/bin/bash:/root:root:0:0:x:root
/sbin/nologin:/bin:bin:1:1:x:bin
/sbin/nologin:/sbin:daemon:2:2:x:daemon
/sbin/nologin:/var/adm:adm:4:3:x:adm

例. 方法二
[root@tx3 ~]# awk 'BEGIN { FS=":" } { i=NF; while (i>=2) {printf $i ":";i--} print $1}' p1
/bin/bash:/root:root:0:0:x:root
/sbin/nologin:/bin:bin:1:1:x:bin
/sbin/nologin:/sbin:daemon:2:2:x:daemon

(4)for循环

语法：
{
for(表达式)
{动作1;...}
}
表达式：分为3部分：
(1)初始化表达式 i=1
(2)测试表达式   i<10
(3)更新测试表达式 i++
语句：
next 处理输入行的下一个输入行
exit 退出
continue 结束本次循环
break 跳出循环


例
[root@tx3 ~]# awk 'BEGIN {FS=":"} {for(i=NF;i>=2;i--) {printf $i ";"};print $1}' p1
/bin/bash;/root;root;0;0;x;root
/sbin/nologin;/bin;bin;1;1;x;bin
/sbin/nologin;/sbin;daemon;2;2;x;daemon
/sbin/nologin;/var/adm;adm;4;3;x;adm
