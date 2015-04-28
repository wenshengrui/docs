	shell 脚本知识总结

第一章 基础知识

1. 脚本编程语言与编译型语言的差异
（1）编译型语言的优点：效率高
	 编译型语言的缺点：多半运作于底层，所处理的是字节、整数、浮点数或其他机器层级的对象

（2）脚本编程语言优点：速度快和解释型语言。
     是由解释器读入程序代码，并将其转换成内部的形式，再执行。
     注：解释器本身就是一般的编译型程序。
	 脚本编程语言缺点：效率低

（3）shell（高级语言）的特性：简单性、可移植性、开发容易

2. shell脚本
（1）计算用户个数：
     [root@newvbox shell]# who | wc -l
（2）shell识别三种基本命令：内建命令、shell函数、外部命令
（3）变量赋值：
str='123456'
str2="12456dfds"
newstr=$str1
（4）定义变量
declare attribute value

用途说明
declare命令是bash的一个内建命令，它可以用来声明shell变量，设置变量的属性（Declare variables and/or give them attributes）。该命令也可以写作typeset。虽然人们很少使用这个命令，如果知道了它的一些用法，就会发现这个命令还是挺有用的。
 
常用参数
格式：declare
格式：typeset
格式：declare -p
格式：typeset -p
显示所有变量的值。
 
格式：declare -p var
格式：typeset -p var
显示指定变量var的值。
 
格式：declare var=value
格式：typeset var=value
格式：var=value
声明变量并赋值。
 
格式：declare -i var
格式：typeset -i var
将变量var定义成整数。在之后就可以直接对表达式求值，结果只能是整数。如果求值失败或者不是整数，就设置为0。
 
格式：declare -r var
格式：typeset -r var
格式：readonly var
将变量var声明为只读变量。只读变量不允许修改，也不允许删除。
 
格式：declare -a var
格式：typeset -a var
将变量var声明为数组变量。但这没有必要。所有变量都不必显式定义就可以用作数组。事实上，在某种意义上，似乎所有变量都是数组，而且赋值给没有下标的变量与赋值给"[0]"相同。
 
格式：declare -f
格式：typeset -f
显示所有自定义函数，包括名称和函数体。
 
格式：declare -F
格式：typeset -F
显示所有自定义函数名称。
 
格式：declare -f func
格式：typeset -f func
只显示指定函数func的函数定义。
 
格式：declare -x var
格式：typeset -x var
格式：export var
将变量var设置成环境变量，这样在随后的脚本和程序中可以使用。
 
格式：declare -x var=value
格式：typeset -x var=value
格式：export var=value
将变量var设置陈环境变量，并赋值为value。
 
使用示例
示例一 declare是内建命令
[root@web ~]#type -a declare 
declare is a shell builtin
[root@web ~]#
[root@jfht ~]#type -a typeset 
typeset is a shell builtin
[root@jfht ~]#
 
示例二 declare -i之后可以直接对表达式求值
[root@web ~]#x=6/3 
[root@web ~]#echo $x 
6/3
[root@web ~]#declare -i x 
[root@web ~]#echo $x    
6/3
[root@web ~]#x=6/3 
[root@web ~]#echo $x 

如果变量被声明成整数，可以把表达式直接赋值给它，bash会对它求值。
[root@jfht ~]#x=error 
[root@jfht ~]#echo $x 

如果变量被声明成整数，把一个结果不是整数的表达式赋值给它时，就会变成0.
[root@jfht ~]#x=3.14 
-bash: 3.14: syntax error: invalid arithmetic operator (error token is ".14")
如果变量被声明成整数，把一个小数（浮点数）赋值给它时，也是不行的。因为bash并不内置对浮点数的支持。 
[root@web ~]#
[root@jfht ~]#declare +i x
此命令的结果是取消变量x的整数类型属性。 
[root@jfht ~]#x=6/3 
[root@jfht ~]#echo $x 
6/3
因为变量x不是整型变量，所以不会自动对表达式求值。可以采用下面两个方式。
[root@jfht ~]#x=$[6/3] 
[root@jfht ~]#echo $x
2
[root@jfht ~]#x=$((6/3)) 
[root@jfht ~]#echo $x  
2
[root@jfht ~]#
 
示例三 声明只读变量
[root@jfht ~]#declare -r r 
[root@jfht ~]#echo $r 

[root@jfht ~]#r=xxx 
-bash: r: readonly variable
[root@jfht ~]#declare -r r=xxx 
-bash: declare: r: readonly variable
[root@jfht ~]#declare +r r   
-bash: declare: r: readonly variable
[root@jfht ~]#
[root@jfht ~]#declare +r r 
-bash: declare: r: readonly variable
[root@jfht ~]#
[root@jfht ~]#unset r 
-bash: unset: r: cannot unset: readonly variable
[root@jfht ~]#
 
示例四 声明数组变量（实际上，任何变量都可以当做数组来操作）
[root@jfht ~]#declare -a names 
[root@jfht ~]#names=Jack 
[root@jfht ~]#echo ${names[0]} 
Jack
[root@jfht ~]#names[1]=Bone 
[root@jfht ~]#echo ${names[1]} 
Bone
[root@jfht ~]#echo ${names} 
Jack
[root@jfht ~]#echo ${names[*]} 
Jack Bone
[root@jfht ~]#echo ${#names} 
4
直接引用names，相当于引用names[0] 
[root@jfht ~]#echo ${#names[*]} 
2
[root@jfht ~]#echo ${names[@]} 
Jack Bone
[root@jfht ~]#echo ${#names[@]} 
2
[root@jfht ~]#declare -p names   
declare -a names='([0]="Jack" [1]="Bone")'
[root@jfht ~]#
 
示例五 显示函数定义
[root@jfht ~]#declare -F 
declare -f add_jar
declare -f add_jar2
declare -f add_jar3
[root@jfht ~]#declare -f 
add_jar ()
{
    [ -e $1 ] && CLASSPATH=$CLASSPATH:$1
}
add_jar2 ()
{
    if [ -e $1 ]; then
        CLASSPATH=$CLASSPATH:$1;
    else
        if [ -e $2 ]; then
            CLASSPATH=$CLASSPATH:$2;
        fi;
    fi
}
add_jar3 ()
{
    if [ -e $1 ]; then
        CLASSPATH=$CLASSPATH:$1;
    else
        if [ -e $2 ]; then
            CLASSPATH=$CLASSPATH:$2;
        else
            if [ -e $3 ]; then
                CLASSPATH=$CLASSPATH:$3;
            fi;
        fi;
    fi
}
[root@jfht ~]#declare -f add_jar 
add_jar ()
{
    [ -e $1 ] && CLASSPATH=$CLASSPATH:$1
}
[root@jfht ~]#declare -F add_jar 
add_jar
[root@jfht ~]#declare -F add_ 
[root@jfht ~]#declare -F add_* 
[root@jfht ~]#declare -F 'add_*' 
[root@jfht ~]#

3. echo 输出
   echo遇到转义序列时，会打印相应的字符。有效的转义序列如下所示：
	
   序列        说明
	\a         警示字符，通常时ASCLL的DEL字符
	\b         退格（Backspace）
	\c         输出中乎略最后的换行字符（Newline）。这个参数之后的任何字符，包括接下来的参数，都会被忽略掉（不打印）。
	\f         清除屏幕（Formfeed）
	\n         换行（Newline）
	\r         回车（Carriage return）
	\t         水平制表符（Horizontal tab）
	\v         垂直制表符（Vertical tab）
	\\         反斜杠字符
	\Oddd      将字符表示成1到3位的八进制数值

4. printf 输出

5. 重定向与管道
（1）以 < 改变标准输入
（2）以 > 改变标准输出
（3）以 >> 附加到文件
（4）以 | 建立管道

转换字符：tr [options] source-char-list replace-char-list


		第二章 查找与替换

1、查找文本 ( grep 查找文本内容 )
（1）grep  普通查找
（2）egrep 扩展式查找 (Extended grep)
（3）fgrep 快速查找 (Fast grep)

2、简单的grep
[root@newvbox shell]# who | grep -F austen

3、正则表达式
grep [ options ……] pattern-spec [ files ……]  显示匹配一个或多个模式的文本行。

注意grep处理下列情况的方式:
<1>.grep是一个搜索程序,它只能搜索匹配一个正则表达式的一行的存在性.
<2>.grep可以对一行采取唯一的动作是把它发送到标准输出. 如果该行不匹配正则表达式,则其不被打印.
<3>.行的选择只基于正则表达式. 行编号或其他准则不能用于选择行.
<4>.grep是一个过滤器. 它可用在管道的左边或右边.
<5>.grep不能用于增加,删除或修改行.
<6>.grep不能用于只打印行的一部分.
<7>.grep不能只读取文件的一部分.
<8>.grep不能基于前面的内容或下一行来选择一行.只有一个缓冲区,它只保存当前行.

GREP家族包括: grep,fgrep,egrep

fgrep:只支持字符串模式,不支持正则表达式.
 grep:只支持数量有限的正则表达式.
egrep:支持大多数的正则表达式,但不是全部.

grep家族的选项:
-b   在每一行前加上所在文件块的编号
-c   只打印匹配模式的行编号记数
-i   在匹配文本时忽略大小写
-l   打印至少有一行匹配模式的文件列表
-n   在每行前显示其行编号
-s   哑模式. 执行其功能,但抑制所有输出
-v   逆向输出. 打印不匹配模式的行
-x   只打印完全匹配模式的行
-f   file 要匹配的字符串列表在文件file中

grep:

一般格式：grep [options]  基本正则表达式 [filename]

注意：基本正则表达式可以为字符串，如果是字符串的时候请加上“”号，否则容易出错


4、sed 的用法：替换文件信息

sed编辑器逐行处理输入，然后把结果发送到屏幕。

-i选项：直接作用源文件，源文件将被修改。
sed命令和选项：

a\	在当前行后添加一行或多行
c\	用新文本替换当前行中的文本
d	删除行
i\	在当前行之前插入文本
h	把模式空间的内容复制到暂存缓冲区
H	把模式空间的内容添加到缓冲区
g	取出暂存缓冲区的内容，将其复制到模式缓冲区
G	取出暂存缓冲区的内容，将其追加到模式缓冲区
l	列出非打印字符
p	打印行
n	读入下一行输入，并从下一条而不是第一条命令对其处理
q	结束或退出sed
r	从文件中读取输入行
!	对所选行以外的行应用所有命令
s	用一个字符串替换另外一个字符串


替换标志：
g	在行内进行全局替换
p	打印行
w	将行写入文件
x	交换暂存缓冲区和模式空间的内容
y	将字符转换成另外一个字符
 

sed例子：
打印：p命令
sed '/abc/p' file  打印file中包含abc的行。默认情况sed把所有行都打印到屏幕，如果某行匹配到模式，则把该行另外再打印一遍
sed  -n '/abc/p' file	和上面一样，只是去掉了sed的默认行为，只会打印匹配的行

删除：d命令

sed '3,$d' file		删除从第3行到最后一行的内容。
sed '$d' file		删除最后一行的内容
sed '/abc/d'		删除包含abc的行。
sed '3d' file		删除第三行的内容

替换：s命令
sed  's/abc/def/g' file		把行内的所有abc替换成def，如果没有g,则只替换行内的第一个abc
sed  -n 's/abc/def/p' file	只打印发生替换的那些行
sed  's/abc/&def/' file		在所有的abc后面添加def（&表示匹配的内容）
sed  -n 's/abc/def/gp' file	把所有的abc替换成def，并打印发生替换的那些行
sed  's#abc#def#g' file		把所有的abc替换成def，跟在替换s后面的字符就是查找串和替换串之间的分割字符，本例中试#

指定行的范围：逗号
sed  -n '/abc/,/def/p' file	打印模式abc到def的行
sed  -n '5/,/def/p' file	打印从第五行到包含def行之间的行。
sed /abd/,/def/s/aaa/bbb/g	修改从模式abc到模式def之间的行，把aaa替换成def

多重编辑-e
sed  -e '1,3d' -e 's/abc/def/g' file	删除1-3行，然后把其余行的abc替换成def

读文件：r命令
sed  '/abc/r newfile' file	在包含abc的行后读入newfile的内容

写文件：w命令  
sed  '/abc/w newfile' file	在包含abc的行写入newfile

追加：a命令     
sed  '/abc/a\def' file	在包含abc的行后新起一行，写入def

插入：i命令     
sed  '/abc/i\def' file	在包含abc的行前新起一行，写入def

修改：c命令   
sed  '/abc/c\def' file	在包含abc的行替换成def，旧文本被覆盖

读取下一行：n命令  
sed  '/abc/{n ; s/aaa/bbb/g;}' file	读取包含abc的行的下一行，替换aaa为bbb

转换：y命令       
sed  'y/abc/ABC' file	将a替换成A，b替换成B，c替换成C（正则表达式元字符不起作用）

退出：q命令
sed  '/abc/{ s/aaa/bbb/ ;q; }' file	在某行包含了abc，把aaa替换成bbb，然后退出sed。
 
暂存和取用：h命令（把模式行存储到暂存缓冲区）和g（取出暂存缓冲区的行并覆盖模式缓冲区）G（取出临时缓冲区的行）命令 
h和g是复制行为(覆盖），H和G表示追加。    

sed  -e '/abc/h'  -e '$G' file	包含abc的行通过h命令保存到暂存缓冲区，在第二条命令汇中，sed读到最后一行$时，G命令从暂存缓冲区中读取一行，追加到模式缓冲区的后面。即所有包含abc的行的最后一行被复制到文件末尾。
sed -e '/abc/{h; d;}'
sed -e '/def/{g; }' file	包含abc的行会移到包含def的行上，并进行覆盖。

暂存和互换：h和x命令 
sed -e '/abc/h' -e '/def/x' file	包含abc的行会被换成def的行。
 
读取文件某行
sed -n '2,3'p file 读取文件的第二行与第三行

5、使用cut选定字段
cut 命令：从一个文本文件或者文本流中提取文本列
cut -d'分隔字符' -f fields <== 用于有特定分隔字符
cut -c 字符区间            <== 用于排列整齐的信息
选项与参数：
-d  ：后面接分隔字符。与 -f 一起使用；
-f  ：依据 -d 的分隔字符将一段信息分割成为数段，用 -f 取出第几段的意思；
-c  ：以字符 (characters) 的单位取出固定字符区间；

6、join 连接字段：（以一个共同的键值，将以存储文件内的记录加以结合）
join命令：将多个文件结合在一起，每个文件里的每条记录都共享一个键值。键值指的是记录中的主字段

7、awk 重新编排字段
awk -F: '{print "User",$1," ==> ",$5 }' /etc/passwd

			第三章 文件处理工具

4.1 排序文本
4.1.1 行的排序

一、Sort命令
sort [OPTION]… [FILE] 对文件按指定的域进行排序
常用选项：
	-b:		按域排序时忽略第一个空格
	-c:		检测文件是否已经排序
	-d:		字典顺序，仅文字数字与空白才有意义
	-f:		不区分字母的大小写
	-g:		以一般的浮点数字进行比较，只适用于GNU版本
	-i:		忽略无法打印的字符
	-m:		将两个已经排序的文件进行合并
	-u:		在排序过程中，删除重复的行
	-o:		保存排序后的文件
	-t:		域分隔符，默认为空格和tab
	-n:		以（整数）数字比较
	-r:		倒序排列
	+n:		按照第n个域进行排序，域从第0个开始计数
	+m.n:	按照第m个域的第n个字符开始开始排序
	-k:		按照第k个域进行排序，域从第1个开始计数

sort -o output.txt  your_file.txt	# 对文件按第一域进行排序，将排序结果保存到output.txt
sort -t: -r +2n your_file.txt		# 对文件按照第2个域进行逆向排序，第二个域为数字类型，同时分割符为:
df | sort -b -r -k5					# 按照磁盘的占用率从高到底进行排序输出

sort -t: -k1,1 /etc/passwd		以用户名排序
sort -t: -k3nr /etc/passwd		反向UID排序
sort -t: -k4n -k3n /etc/passwd	以GID与UID排序
sort sort.sh | uniq -c			计数唯一的，排序后的记录
sort sort.sh | uniq -d			仅显示重复的记录
sort sort.sh | uniq -u			仅显示为重复的记录

二、字符统计工具 wc
echo This is a test of the eee | wc     ==> 1       7      26
echo This is a test of the eee | wc -c  ==> 26		统计字节数
echo This is a test of the eee | wc -l  ==> 1		统计行数
echo This is a test of the eee | wc -w  ==> 7		统计字数

三、提取开头或结尾数行
head -n 2 filename 前两行
tail -n 2 filename 后两行

第四章 管道的神奇魔力

第五章 变量、判断、重复动作

export 修改或打印环境变量，readonly 则使得变量不得修改
unset 从当前shell删除变量与函数
unset -f 删除指定的函数
unset -v 删除指定的变量


exec	执行函数
printf	打印字符串

		第六章 字符串 
1、字符串提取 substr(string, start, length)
substr('abcdefgh', 2,4)

2、大小写转换 tolower(string) 将字符串转换为小写字母
              toupper(string) 将字符串转换为大写字母

3、字符串查找 index(string, find_str) 返回要查找的字符串的开始位置或 0

function rindex(string,find,k,ns,nf)
{
	ns = length(string)
	nf = length(find)

	for(k= ns + 1- nf;k >=1;k--)
	{
		if (substr(string, k, nf) == find)
			return k
		return 0
	}
}

4、字符串匹配
match(string, regexp) reqexp 正则表达式

5、字符串替换 
awk 在字符串替换的功能上，提供两函数：
	sub(regexp, replacement, target)
	gsub(regexp, replacement, target)

	sub(/^ *[a-z]+ *= *"/, "", value)

6、字符串分割
split(string, array, regexp)

function strSubStr()
{
	print *\nField separator = FS = \"" FS "\""
	n = split($0,parts)
	for(k=1;k<=n;k++)
		print "parts[" k "] = \"" parts[k] "\""

	print *\nField separator = \"[ ]\""
	n = split($0,parts,"[ ]")
	for(k=1;k<=n;k++)
		print "parts[" k "] = \"" parts[k] "\""

	print *\nField separator = \":\""
	n = split($0,parts,":")
	for(k=1;k<=n;k++)
		print "parts[" k "] = \"" parts[k] "\""
	
	print ""
}

7、格式化函数 printf() 与 sprintf()

8、数值函数

function irand(low,high,n)
{
	low=int(low)
	high=int(high)

	if(low >= high)
		return (low)

	do
		n=low + int(rand() * (high+1-low))
	while(( n < low ) || (n > high))

	return (n)
}

9、快速寻找文件

localte filename 
find 文件名

df 查询文件系统的信息
du 显示一个或多个目录树内的空间使用率

10、文件比较
cmp与diff 
cmp filename1 filename2

11、数字签名验证

12、休眠循环脚本 looper

13、定时执行 crontab 在指定时间执行脚本

你好美女，最近工作压力大，我想找个人放纵一回，可以找你吗


主要Shell内置命令

Shell有很多内置在其源代码中的命令。这些命令是内置的，所以Shell不必到磁盘上搜索它们，执行速度因此加快。不同的Shell内置命令有所不同。

A.2.1  bash内置命令

.：执行当前进程环境中的程序。同source。

. file：dot命令从文件file中读取命令并执行。

: 空操作，返回退出状态0。

alias：显示和创建已有命令的别名。

bg：把作业放到后台。

bind：显示当前关键字与函数的绑定情况，或将关键字与readline函数或宏进行绑定。

break：从最内层循环跳出。

builtin [sh-builtin [args]]：运行一个内置Shell命令，并传送参数，返回退出状态0。当一个函数与一个内置命令同名时，该命令将很有用。

cd [arg]：改变目录，如果不带参数，则回到主目录，带参数则切换到参数所指的目录。

command comand [arg]：即使有同名函数，仍然执行该命令。也就是说，跳过函数查找。

declare [var]：显示所有变量，或用可选属性声明变量。

dirs：显示当前记录的目录（pushd的结果）。

disown：从作业表中删除一个活动作业。

echo [args]：显示args并换行。

enable：启用或禁用Shell内置的命令。

eval [args]：把args读入Shell，并执行产生的命令。

exec command：运行命令，替换掉当前Shell。

exit [n]：以状态n退出Shell。

export [var]：使变量可被子Shell识别。

fc：历史的修改命令，用于编辑历史命令。

fg：把后台作业放到前台。

getopts：解析并处理命令行选项。

hash：控制用于加速命令查找的内部哈希表。

help [command]：显示关于内置命令的有用信息。如果指定了一个命令，则将显示该命令的详细信息。

history：显示带行号的命令历史列表。

jobs：显示放到后台的作业。

kill [-signal process]：向由PID号或作业号指定的进程发送信号。输入kill-l查看信号列表。

let：用来计算算术表达式的值，并把算术运算的结果赋给变量。

local：用在函数中，把变量的作用域限制在函数内部。

logout：退出登录Shell。

popd：从目录栈中删除项。

pushd：向目录栈中增加项。

pwd：打印出当前的工作目录。

read [var]：从标准输入读取一行，保存到变量var中。

readonly [var]：将变量var设为只读，不允许重置该变量。

return [n]：从函数中退出，n是指定给return命令的退出状态值。

set：设置选项和位置参量。

shift [n]：将位置参量左移n次。

stop pid：暂停第pid号进程的运行。

suspend：终止当前Shell的运行（对登录Shell无效）。

test：检查文件类型，并计算条件表达式。

times：显示由当前Shell启动的进程运行所累计用户时间和系统时间。

trap [arg] [n]：当Shell收到信号n（n为0、1、2或15）时，执行arg。

type [command]：显示命令的类型，例如：pwd是Shell的一个内置命令。

typeset：同declare。设置变量并赋予其属性。

ulimit：显示或设置进程可用资源的最大限额。

umask [八进制数字]：用户文件关于属主、属组和其他用户的创建模式掩码。

unalias：取消所有的命令别名设置。

unset [name]：取消指定变量的值或函数的定义。

wait [pid#n]：等待pid号为n的后台进程结束，并报告它的结束状态。

