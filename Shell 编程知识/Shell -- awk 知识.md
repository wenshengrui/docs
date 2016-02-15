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
<<<<<<< HEAD
=======
<<<<<<< HEAD
=======




简单使用：
awk ：对于文件中一行行的独处来执行操作 。
awk -F ：'{print $1,$4}'   :使用‘：’来分割这一行，把这一行的第一第四个域打印出来 。
 
 
详细介绍：
AWK命令介绍
 
awk语言的最基本功能是在文件或字符串中基于指定规则浏览和抽取信息，awk抽取信息后，才能进行其他文本操作，完整的awk脚本通常用来格式化文本文件中的信息
 
1.   调用awk:
 
第一种命令行方式，如:
 
awk [-Field-separator] 'commands' input-file(s)
 
这里commands是真正的awk命令，[-F域分隔符]是可选的，awk默认使用空格分隔，因此如果要浏览域间有空格的文本，不必指定这个选项，但如果浏览如passwd文件，此文件各域使用冒号作为分隔符，则必须使用-F选项:   awk -F : 'commands' input-file
 
   第二种，将所有awk命令插入一个文件，并使awk程序可执行，然后用awk命令解释器作为脚本的首行，以便通过键入脚本名称来调用它
 
   第三种，将所有awk命令插入一个单独文件，然后调用，如: 
 
awk -f awk-script-file input-file
 
-f选项指明在文件awk-script-file的awk脚本，input-file是使用awk进行浏览的文件名
 
2.   awk脚本:
 
    awk脚本由各种操作和模式组成，根据分隔符(-F选项)，默认为空格，读取的内容依次放置到对应的域中，一行一行记录读取，直到文件尾
 
2.1.      模式和动作   
 
任何awk语句都是由模式和动作组成，在一个awk脚本中可能有许多语句。模式部分决定动作语句何时触发及触发事件。动作即对数据进行的操作，如果省去模式部分，动作将时刻保持执行状态
 
    模式可以是任何条件语句或复合语句或正则表达式，模式包含两个特殊字段BEGIN和END，使用BEGIN语句设置计数和打印头，BEGIN语句使用在任何文本浏览动作之前，之后文本浏览动作依据输入文件开始执行;END语句用来在awk完成文本浏览动作后打印输出文本总数和结尾状态标志，有动作必须使用{}括起来
 
    实际动作在大括号{}内指明，常用来做打印动作，但是还有更长的代码如if和循环looping语句及循环退出等，如果不指明采取什么动作，awk默认打印出所有浏览出的记录
 
2.2.     域和记录:
 
awk执行时，其浏览标记为$1，$2...$n，这种方法称为域标记。使用$1，$3表示参照第1和第3域，注意这里使用逗号分隔域，使用$0表示使用所有域。例如:
 
awk '{print $0}' temp.txt > sav.txt  
 
表示打印所有域并把结果重定向到sav.txt中
 
awk '{print $0}' temp.txt|tee sav.txt 
 
 和上例相似，不同的是将在屏幕上显示出来
 
awk '{print $1,$4}' temp.txt
 
   只打印出第1和第4域
 
awk 'BEGIN {print "NAME  GRADE\n----"} {print $1"\t"$4}' temp.txt 
 
表示打信息头，即输入的内容的第一行前加上"NAME  GRADE\n-------------"，同时内容以tab分开
 
awk 'BEGIN {print "being"} {print $1} END {print "end"}' temp 
 
同时打印信息头和信息尾
 
2.3 条件操作符:
 
    <、<=、==、!=、>=、~匹配正则表达式、!~不匹配正则表达式
 
    匹配:awk '{if ($4~/ASIMA/) print $0}' temp 表示如果第四个域包含ASIMA，就打印整条
 
    精确匹配:awk '$3=="48" {print $0}' temp    只打印第3域等于"48"的记录
 
    不匹配:  awk '$0 !~ /ASIMA/' temp      打印整条不包含ASIMA的记录
 
    不等于:  awk '$1 != "asima"' temp
 
    小于:    awk '{if ($1<$2) print $1 "is smaller"}' temp
 
    设置大小写: awk '/[Gg]reen/' temp      打印整条包含Green，或者green的记录
 
    任意字符: awk '$1 ~/^...a/' temp    打印第1域中第四个字符是a的记录，符号’^’代表行首，符合’.’代表任意字符
 
    或关系匹配: awk '$0~/(abc)|(efg)/' temp   使用|时，语句需要括起来
 
    AND与关系:  awk '{if ( $1=="a" && $2=="b" ) print $0}' temp
 
    OR或关系:   awk '{if ($1=="a" || $1=="b") print $0}' temp
 
2.4 awk内置变量:
 
ARGC		命令行参数个数			NF	浏览记录的域个数
AGRV		命令行参数排列			NR	已读的记录数   
ENVIRON		支持队列中系统环境变量的使用	OFS	输出域分隔符
FILENAME	awk浏览的文件名			ORS	输出记录分隔符
FNR		浏览文件的记录数		RS	控制记录分隔符
FS		设置输入域分隔符，同- F选项	NF	浏览记录的域个数
 
例: 
awk 'END {print NR}' temp    在最后打印已读记录条数
awk '{print NF，NR，$0} END {print FILENAME}' temp
awk '{if (NR>0 && $4~/Brown/) print $0}' temp  至少存在一条记录且包含Brown
NF的另一用法:  echo $PWD | awk -F/ '{print $NF}'   显示当前目录名
 
2.5 awk操作符:
 
在awk中使用操作符，基本表达式可以划分成数字型、字符串型、变量型、域及数组元素
 
设置输入域到变量名: 
awk '{name=$1;six=$3; if (six=="man") print name " is " six}' temp

域值比较操作: 
awk 'BEGIN {BASE="27"} {if ($4<BASE) print $0}' temp

修改数值域取值:(原输入文件不会被改变)
awk '{if ($1=="asima") $6=$6-1;print $1，$6，$7}' temp

修改文本域: 
awk '{if ($1=="asima) ($1=="desc");print $1}' temp
 
只显示修改记录:(只显示所需要的，区别上一条命令，注意{})
 
awk '{if ($1=="asima) {$1=="desc";print$1}}' temp
 
创建新的输出域:
 
awk '{$4=$3-$2; print $4}' temp
 
统计列值:
 
awk '(tot+=$3);END {print tot}' temp           会显示每列的内容
 
awk '{(tot+=$3)};END {print tot}' temp         只显示最后的结果
 
    文件长度相加:
 
ls -l|awk '/^[^d]/ {print $9"\t"$5} {tot+=$5} END{print "totKB:" tot}'
 
    只列出文件名:
 
ls -l|awk '{print $9}'     常规情况文件名是第9域
 
2.6.     awk内置字符串函数:
 
gsub(r，s)           在整个$0中用s替代r
 
awk 'gsub(/name/，"xingming") {print $0}' temp
 
gsub(r，s，t)         在整个t中用s替代r
 
index(s，t)          返回s中字符串t的第一位置
 
awk 'BEGIN {print index("Sunny"，"ny")}' temp     返回4
 
length(s)           返回s的长度
 
match(s，r)          测试s是否包含匹配r的字符串
 
awk '$1=="J.Lulu" {print match($1，"u")}' temp    返回4
 
split(s，a，fs)       在fs上将s分成序列a
 
awk 'BEGIN {print split("12#345#6789"，myarray，"#")"'
 
返回3，同时myarray[1]="12"， myarray[2]="345"， myarray[3]="6789"
 
sprint(fmt，exp)     返回经fmt格式化后的exp
sub(r，s)   从$0中最左边最长的子串中用s代替r(只更换第一遇到的匹配字符串)
substr(s，p)         返回字符串s中从p开始的后缀部分
substr(s，p，n)       返回字符串s中从p开始长度为n的后缀部分
 
2.7 printf函数的使用:
 
字符转换: echo "65" |awk '{printf "%c\n"，$0}'    输出A
 
            awk 'BEGIN {printf "%f\n"，999}'        输出999.000000
 
格式化输出:awk '{printf "%-15s %s\n"，$1，$3}' temp 将第一个域全部左对齐显示
 
2.8.     其他awk用法:
 
    向一行awk命令传值:
 
awk '{if ($5<AGE) print $0}' AGE=10 temp
 
who | awk '{if ($1==user) print $1 " are in " $2 ' user=$LOGNAME 使用环境变量
 
awk脚本命令:
 
开头使用 !/bin/awk -f  ，如果没有这句话自含脚本将不能执行，例子:
 
!/bin/awk -f
 
# all comment lines must start with a hash '#'
# name: student_tot.awk
# to call: student_tot.awk grade.txt
# prints total and average of club student points
# print a header first

BEGIN
{
	print "Student    Date   Member No.  Grade  Age  Points  Max"
	print "Name  Joined Gained  Point Available"
	print"========================================================="
}
 
# let's add the scores of points gained
 
(tot+=$6);
 
# finished processing now let's print the total and average point
 
END
 
{
    print "Club student total points :" tot
    print "Average Club Student points :" tot/N
 
}
 
2.9.     awk数组:
 
awk的循环基本结构
 
For (element in array) print array[element]
 
awk 'BEGIN {record="123#456#789";split(record，myarray，"#")} 
 
END { for (i in myarray) {print myarray[i]} }
 
 
3.0  awk中自定义语句
 
一.条件判断语句(if)
if(表达式) #if ( Variable in Array )
语句1
else
语句2
格式中"语句1"可以是多个语句，如果你为了方便Unix awk判断也方便你自已阅读，你最好将多个语句用{}括起来。Unix awk分枝结构允许嵌套，其格式为：
if(表达式)
{语句1}
else if(表达式)
{语句2}
else
{语句3}
[xifj@localhost nginx]# awk 'BEGIN{ 
test=100;
if(test>90)
{
    print "very good";
}
else if(test>60)
{
    print "good";
}
else
{
    print "no pass";
}
}'
very good
 
每条命令语句后面可以用“；”号结尾。
 
二.循环语句(while,for,do)
1.while语句
格式：
while(表达式)
{语句}
例子：
[xifj@localhost nginx]# awk 'BEGIN{ 
test=100;
total=0;
while(i<=test)
{
    total+=i;
    i++;
}
print total;
}'
5050
2.for 循环
for循环有两种格式：
格式1：
for(变量 in 数组)
{语句}
例子：
[xifj@localhost nginx]# awk 'BEGIN{ 
for(k in ENVIRON)
{
    print k"="ENVIRON[k];
}
}'
AWKPATH=.:/usr/share/awk
OLDPWD=/home/web97
SSH_ASKPASS=/usr/libexec/openssh/gnome-ssh-askpass
SELINUX_LEVEL_REQUESTED=
SELINUX_ROLE_REQUESTED=
LANG=zh_CN.GB2312
。。。。。。
说明：ENVIRON 是awk常量，是子典型数组。
格式2：
for(变量;条件;表达式)
{语句}
例子：
[xifj@localhost nginx]# awk 'BEGIN{ 
total=0;
for(i=0;i<=100;i++)
{
    total+=i;
}
print total;
}'
5050
3.do循环
格式：
do
{语句}while(条件)
例子：
[xifj@localhost nginx]# awk 'BEGIN{ 
total=0;
i=0;
do
{
	total+=i;
	i++;
}while(i<=100)
	print total;
}'
5050
 
以上为awk流程控制语句，从语法上面大家可以看到，与c语言是一样的。有了这些语句，其实很多shell程序都可以交给awk，而且性能是非常快的。
break	当 break 语句用于 while 或 for 语句时，导致退出程序循环。
continue	当 continue 语句用于 while 或 for 语句时，使程序循环移动到下一个迭代。
next	能能够导致读入下一个输入行，并返回到脚本的顶部。这可以避免对当前输入行执行其他的操作过程。
exit	语句使主输入循环退出并将控制转移到END,如果END存在的话。如果没有定义END规则，或在END中应用exit语句，则终止脚本的执行。
>>>>>>> ea93dcd8ef2bb27bf22cc3b1e28b62172ea71ecd
>>>>>>> 0d324f7bcf6c650bee42b6e46c38231f0dbcc240
