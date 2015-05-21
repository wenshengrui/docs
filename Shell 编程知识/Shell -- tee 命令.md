	Linux Shell脚本编程－－tee命令

用途说明
在执行Linux命令时，我们可以把输出重定向到文件中，比如 ls >a.txt，这时我们就不能看到输出了，如果我们既想把输出保存到文件中，又想在屏幕上看到输出内容，就可以使用tee命令了。tee命令读取标准输入，把这些内容同时输出到标准输出和（多个）文件中（read from standard input and write to standard output and files. Copy standard input to each FILE, and also to standard output. If a FILE is -, copy again to standard output.）。在info tee中说道：tee命令可以重定向标准输出到多个文件（`tee': Redirect output to multiple files. The `tee' command copies standard input to standard output and also to any files given as arguments.  This is useful when you want not only to send some data down a pipe, but also to save a copy.）。要注意的是：在使用管道线时，前一个命令的标准错误输出不会被tee读取。
 
常用参数
格式：tee
只输出到标准输出，因为没有指定文件。
 
格式：tee file
输出到标准输出的同时，保存到文件file中。如果文件不存在，则创建；如果已经存在，则覆盖之。（If a file being written to does not already exist, it is created. If a file being written to already exists, the data it previously
contained is overwritten unless the `-a' option is used.）
 
格式：tee -a file
输出到标准输出的同时，追加到文件file中。如果文件不存在，则创建；如果已经存在，就在末尾追加内容，而不是覆盖。
 
格式：tee -
输出到标准输出两次。（A FILE of `-' causes `tee' to send another copy of input to standard output, but this is typically not that useful as the copies are interleaved.）
 
格式：tee file1 file2 -
输出到标准输出两次，同时保存到file1和file2中。
 
使用示例
示例一 tee命令与重定向的对比
[root@web ~]# seq 5 >1.txt 
[root@web ~]# cat 1.txt 
1
2
3
4
5
[root@web ~]# cat 1.txt >2.txt 
[root@web ~]# cat 1.txt | tee 3.txt 
1
2
3
4
5
[root@web ~]# cat 2.txt 
1
2
3
4
5
[root@web ~]# cat 3.txt 
1
2
3
4
5
[root@web ~]# cat 1.txt >>2.txt 
[root@web ~]# cat 1.txt | tee -a 3.txt 
1
2
3
4
5
[root@web ~]# cat 2.txt 
1
2
3
4
5
1
2
3
4
5
[root@web ~]# cat 3.txt 
1
2
3
4
5
1
2
3
4
5
[root@web ~]#
 
示例二 使用tee命令重复输出字符串
[root@web ~]# echo 12345 | tee 
12345
[root@web ~]# echo 12345 | tee - 
12345
12345
[root@web ~]# echo 12345 | tee - - 
12345
12345
12345
[root@web ~]# echo 12345 | tee - - - 
12345
12345
12345
12345
[root@web ~]# echo 12345 | tee - - - - 
12345
12345
12345
12345
12345
[root@web ~]#
[root@web ~]# echo -n 12345 | tee
12345[root@web ~]# echo -n 12345 | tee - 
1234512345[root@web ~]# echo -n 12345 | tee - -
123451234512345[root@web ~]# echo -n 12345 | tee - - -
12345123451234512345[root@web ~]# echo -n 12345 | tee - - - -
1234512345123451234512345[root@web ~]#
 
示例三 使用tee命令把标准错误输出也保存到文件
[root@web ~]# ls "*" 
ls: *: 没有那个文件或目录
[root@web ~]# ls "*" | tee - 
ls: *: 没有那个文件或目录
[root@web ~]# ls "*" | tee ls.txt 
ls: *: 没有那个文件或目录
[root@web ~]# cat ls.txt 
[root@web ~]# ls "*" 2>&1 | tee ls.txt 
ls: *: 没有那个文件或目录
[root@web ~]# cat ls.txt 
ls: *: 没有那个文件或目录
示例四 列出文本内容，同时复制3份复本
列出文本文件slayers.story的内容，同时复制3份副本，文件名称分别为ss-copy1、ss-copy2、ss-copy3：
[root@web ~]#  cat slayers.story |tee ss-copy1 ss-copy2 ss-copy3