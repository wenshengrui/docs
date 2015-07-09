	Linux Shell脚本编程－－比较运算符

运算符			描述				示例

文件比较运算符
-e filename		如果 filename 存在，则为真		[ -e /var/log/syslog ]
-d filename		如果 filename 为目录，则为真		[ -d /tmp/mydir ]
-f filename		如果 filename 为常规文件，则为真	[ -f /usr/bin/grep ]
-L filename		如果 filename 为符号链接，则为真	[ -L /usr/bin/grep ]
-r filename		如果 filename 可读，则为真		[ -r /var/log/syslog ]
-w filename		如果 filename 可写，则为真		[ -w /var/mytmp.txt ]
-x filename		如果 filename 可执行，则为真		[ -x /usr/bin/grep ]
filename1 -nt filename2	如果 filename1 比 filename2 新，则为真	[ /tmp/install/etc/services -nt /etc/services ]
filename1 -ot filename2	如果 filename1 比 filename2 旧，则为真	[ /boot/bzImage -ot arch/i386/boot/bzImage ]

字符串比较运算符（请注意引号的使用，这是防止空格扰乱代码的好方法）
-z string		如果 string 长度为零，则为真		[ -z "$myvar" ]
-n string		如果 string 长度非零，则为真		[ -n "$myvar" ]
string1 = string2	如果 string1 与 string2 相同，则为真	[ "$myvar" = "one two three" ]
string1 != string2	如果 string1 与 string2 不同，则为真	[ "$myvar" != "one two three" ]

算术比较运算符
num1 -eq num2		等于					[ 3 -eq $mynum ]
num1 -ne num2		不等于					[ 3 -ne $mynum ]
num1 -lt num2		小于					[ 3 -lt $mynum ]
num1 -le num2		小于或等于				[ 3 -le $mynum ]
num1 -gt num2		大于					[ 3 -gt $mynum ]
num1 -ge num2		大于或等于				[ 3 -ge $mynum ]

测试命令

test命令用于检查某个条件是否成立，它可以进行数值、字符和文件3个方面的测试，其测试符和相应的功能分别如下。

（1）数值测试：
　　-eq 等于则为真。
　　-ne 不等于则为真。
　　-gt 大于则为真。
　　-ge 大于等于则为真。
　　-lt 小于则为真。
　　-le 小于等于则为真。


（2）字串测试：
　　= 等于则为真。
　　!= 不相等则为真。
　　-z字串 字串长度为零则为真。
　　-n字串 字串长度非零则为真。
 
（3）文件测试：
　　-e文件名 如果文件存在则为真。
　　-r文件名 如果文件存在且可读则为真。
　　-w文件名 如果文件存在且可写则为真。
　　-x文件名 如果文件存在且可执行则为真。
　　-s文件名 如果文件存在且至少有一个字符则为真。
    -z文件名 文件存在且长度为0返回真。
　　-d文件名 如果文件存在且为目录则为真。
　　-f文件名 如果文件存在且为普通文件则为真。
　　-c文件名 如果文件存在且为字符型特殊文件则为真。
　　-b文件名 如果文件存在且为块特殊文件则为真
    -o文件名 如果文件属于用户本人返回真。