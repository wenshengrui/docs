18859689908

shell -- 文件操作
1、列出文件
ls [option] [path] [file]

2、列出类型
(1) 普通文件
ls -l /usr/local

(2) 目录
文件名只有在目录文件中才能找到。每个子目录或文件都在其父目录中拥有一条记录，该记录包含以下两部分：
1> 文件名
2> 文件或者目录的唯一标识符（即inode节点）

(3) 伪文件：与普通文件或目录不同，伪文件不是用来存储数据的。

3、文件的权限

4、查找文件
(1)find 命令
find path test action (path:搜索的路径，多个路径用空格隔开；test 测试条件，多个条件时用空格隔开；action 对于搜索结果要执行的操作，多个还是空格隔开)

(2) find 命令：路径
find /usr/local

(3)find 命令：测试

(4)find 命令：使用 ！运算符对测试求反

(5)find 命令：处理文件权限错误信息

(6)find 命令：动作

5、比较文件
(1) comm 比较文件
comm [option] ..... file1 file2
option ：
	-1: 不显示第1个文件中独有的文本行
	-2：不显示第2个文件中独有的文本行
	-3：不显示两个文件中共同的文本行
	--check-order：检查参与比较的两个文件是否已经排序
	--nocheck-order：不检查参与比较的两个文件是否已经排序

(1) diff 比较文件
comm [option] ..... files 
参数介绍：
	option ：
		-c: 输出包含上下文环境的格式
		-u：以统一格式显示文件的不同
		-y：以并列的方式显示文件的异同之处
	files 要比较的文件列表

6、重定向
(1)输出重定向
重定向操作符：>  >>
ls -la /usr/local > files.txt
cmd [n]> file
ls -la /usr/local 1> files.txt
ls -la /usr/local &> files.txt
cmd 为shell命令；n 为一个整数，文件描述符； > 重定向操作符；& 表示标准输出和标准错误
注意：文件描述符与重定向操作符之间没有空格

(2)输入重定向：
重定向操作符：<
cmd < files

cmd << delimiter
document
delimiter





1729570350