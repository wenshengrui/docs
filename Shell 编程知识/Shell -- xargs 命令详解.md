	Linux Shell脚本编程－－xargs命令详解

xargs是给命令传递参数的一个过滤器，也是组合多个命令的一个工具。它把一个数据流分割为一些足够小的块，以方便过滤器和命令进行处理。通常情况下，xargs从管道或者stdin中读取数据，但是它也能够从文件的输出中读取数据。xargs的默认命令是echo，这意味着通过管道传递给xargs的输入将会包含换行和空白，不过通过xargs的处理，换行和空白将被空格取代。
xargs 是一个强有力的命令，它能够捕获一个命令的输出，然后传递给另外一个命令，下面是一些如何有效使用xargs 的实用例子。
1. 当你尝试用rm 删除太多的文件，你可能得到一个错误信息：/bin/rm Argument list too long. 用xargs 去避免这个问题
find ~ -name ‘*.log’ -print0 | xargs -0 rm -f

2. 获得/etc/ 下所有*.conf 结尾的文件列表，有几种不同的方法能得到相同的结果，下面的例子仅仅是示范怎么实用xargs ，在这个例子中实用 xargs将find 命令的输出传递给ls -l
# find /etc -name "*.conf" | xargs ls –l

3. 假如你有一个文件包含了很多你希望下载的URL, 你能够使用xargs 下载所有链接
# cat url-list.txt | xargs wget –c

4. 查找所有的jpg 文件，并且压缩它
# find / -name *.jpg -type f -print | xargs tar -cvzf images.tar.gz

5. 拷贝所有的图片文件到一个外部的硬盘驱动 
# ls *.jpg | xargs -n1 -i cp {} /external-hard-drive/directory

EXAMPLES
find /tmp -name core -type f -print | xargs /bin/rm -f
Find files named core in or below the directory /tmp and delete them.  Note that this will work incorrectly if there are any filenames containing newlines or spaces.

find /tmp -name core -type f -print0 | xargs -0 /bin/rm -f
Find  files  named core in or below the directory /tmp and delete them, processing filenames in such a way that file or directory names containing spaces or newlines are correctly handled.

find /tmp -depth -name core -type f -delete
Find files named core in or below the directory /tmp and delete them, but more efficiently than in the previous example (because we avoid the need to use fork(2) and exec(2) to launch rm and we don't need the extra xargs process).

cut -d: -f1 < /etc/passwd | sort | xargs echo
Generates a compact listing of all the users on the system.

xargs sh -c 'emacs "$@" < /dev/tty' emacs
Launches the minimum number of copies of Emacs needed, one after the other, to edit the files listed on xargs' standard input.  This example achieves the same effect as BSD's -o option, but in a more flexible and portable way.
 
 
Xargs用法详解
1. 简介
  之所以能用到这个命令，关键是由于很多命令不支持|管道来传递参数，而日常工作中有有这个必要，所以就有了xargs命令，例如：
find /sbin -perm +700 |ls -l       这个命令是错误的
find /sbin -perm +700 |xargs ls -l   这样才是正确的
xargs 可以读入 stdin 的资料，并且以空白字元或断行字元作为分辨，将 stdin 的资料分隔成为 arguments 。 因为是以空白字元作为分隔，所以，如果有一些档名或者是其他意义的名词内含有空白字元的时候， xargs 可能就会误判了～他的用法其实也还满简单的！就来看一看先！
2. 选项解释
-0 当sdtin含有特殊字元时候，将其当成一般字符，想/'空格等
例如：root@localhost:~/test#echo "//"|xargs  echo 
      root@localhost:~/test#echo "//"|xargs -0 echo 
       /
-a file 从文件中读入作为sdtin，（看例一）
-e flag ，注意有的时候可能会是-E，flag必须是一个以空格分隔的标志，当xargs分析到含有flag这个标志的时候就停止。（例二）
-p 当每次执行一个argument的时候询问一次用户。（例三）
-n num 后面加次数，表示命令在执行的时候一次用的argument的个数，默认是用所有的。（例四）
-t 表示先打印命令，然后再执行。（例五）
-i 或者是-I，这得看linux支持了，将xargs的每项名称，一般是一行一行赋值给{}，可以用{}代替。（例六）
-r no-run-if-empty 当xargs的输入为空的时候则停止xargs，不用再去执行了。（例七）
-s num 命令行的最好字符数，指的是xargs后面那个命令的最大命令行字符数。（例八）
 
-L  num Use at most max-lines nonblank input lines per command line.-s是含有空格的。
-l  同-L
-d delim 分隔符，默认的xargs分隔符是回车，argument的分隔符是空格，这里修改的是xargs的分隔符（例九）
-x exit的意思，主要是配合-s使用。
-P 修改最大的进程数，默认是1，为0时候为as many as it can ，这个例子我没有想到，应该平时都用不到的吧。
3. 应用举例
例一：
root@localhost:~/test#cat test 
#!/bin/sh
echo "hello world/n"
root@localhost:~/test#xargs -a test echo
#!/bin/sh echo hello world/n
root@localhost:~/test#
例二：
root@localhost:~/test#cat txt
/bin tao shou kun
root@localhost:~/test#cat txt|xargs -E 'shou' echo
/bin tao
root@localhost:~/test#
例三：
root@localhost:~/test#cat txt|xargs -p echo
echo /bin tao shou kun ff ?...y
/bin tao shou kun ff
例四：
root@localhost:~/test#cat txt|xargs -n1 echo
/bin
tao
shou
kun
root@localhost:~/test3#cat txt|xargs  echo
/bin tao shou kun
例五：
root@localhost:~/test#cat txt|xargs -t echo
echo /bin tao shou kun 
/bin tao shou kun
例六：
$ ls | xargs -t -i mv {} {}.bak
例七：
root@localhost:~/test#echo ""|xargs -t mv
mv 
mv: missing file operand
Try `mv --help' for more information.
root@localhost:~/test#echo ""|xargs -t -r  mv
root@localhost:~/test#
（直接退出）
例八：
root@localhost:~/test#cat test |xargs -i -x  -s 14 echo "{}"
exp1
exp5
file
xargs: argument line too long
linux-2
root@localhost:~/test#
例九：
root@localhost:~/test#cat txt |xargs -i -p echo {}
echo /bin tao shou kun ?...y
root@localhost:~/test#cat txt |xargs -i -p -d " " echo {}
echo /bin ?...y
echo tao ?.../bin
y
echo shou ?...tao
再如：
root@localhost:~/test#cat test |xargs -i -p -d " " echo {}
echo exp1
exp5
file
linux-2
ngis_post
tao
test
txt
xen-3
 ?...y
root@localhost:~/test#cat test |xargs -i -p echo {}
echo exp1 ?...y
echo exp5 ?...exp1
y
echo file ?...exp5
y