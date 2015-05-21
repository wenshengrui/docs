	 Linux Shell脚本编程－－Head/Tail命令详解

head 与 tail 就像它的名字一样的浅显易懂，它是用来显示开头或结尾某个数量的文字区块，head 用来显示档案的开头至标准输出中，而 tail 想当然尔就是看档案的结尾，看看下面的范例：
## (1)
displays the first 6 lines of a file
head -6 readme.txt
## (2)
displays the last 25 lines of a file
tail -25 mail.txt
范例一是显示档案的前 6 行，范例二则是显示档案最后的 25 行。
而下面的范别，结合 了 head 与 tail 的指令，显示档案的第 11 行到第 20 行：
##(3)
head -20 file | tail -10
在 tail 的使用手册页中显示了比 head 还多的可用参数，其中有一个很好用的参数 " -f "，使用此参数时，tail 不会回传结束信号，除非我们去自行去中断它；相反的，它会一直等待一段时间，一直到他发现资料自它最后一次被读取后，又被加入新的一行时：
## (4)
display ongoing updates to the given
## log file
tail -f /usr/tmp/logs/daemon_log.txt
上述范例可以动态显示该 log 文件的动态更新。
假设该服务 程序是一直不断的加入动态资料到 /usr/adm/logs/daemon_log.txt 的 log 文件里，在命令列控制窗口中使用 tail -f，它将会以一定的时间实时追踪该档的所有更新。 ( -f 的只有在其输入为档案时才能使用 )。
假如你在 tail 后下了多个档案参数，你便能在同一个窗口内一次追踪数个 log 档：
## track the mail log and the server error log
## at the same time.
tail -f /var/log/mail.log /var/log/apache/error_log
tac -- 反过来串连?!
cat 倒过来怎么拼 ?这就是 tac 的功能， 它是把档案的顺序内容反过来串连用的，那么 ~ 它都用在什么状况下呢 ? 任何须要以后进先出的顺序重新排列组件的工作都用得上它 ! 以下面的指令来说，便是以自最后建立的到最先建立的顺序，列出三个最新建的使用者帐号：
## (5)
last 3 /etc/passwd records - in reverse
$ tail -3 /etc/passwd | tac
curly:x:1003:100:3rd Stooge:/homes/curly:/bin/ksh
larry:x:1002:100:2nd Stooge:/homes/larry:/bin/ksh
moe:x:1001:100:1st Stooge:/homes/moe:/bin/ksh
 
head与tail一套组合拳：
head -n 10030 application.log | tail -n +10000
输出application.log的10000行至10030行内容。