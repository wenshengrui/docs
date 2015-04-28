Shell -- 子shell与进程处理
1、子shell
cat /etc/passwd
登录名：x：用户ID：用户组ID：设备信息：用户主目录：默认shell程序
root:x:0:0:root:/root:/bin/bash
注：如果 默认shell程序不存在、不合法或执行失败，则无法登录主机

2、内部命令、保留字和外部命令
(1)内部命令：cd、pwd等
(2)保留字：if,while,for,then
(3)外部命令：ls, at, host

3、在子shell中执行命
(1) 圆括号结构 (cmd; cmd; cmd;.....)

#!/bin/sh
echo "Before starting subshell"
(
	count=1
	while [ $count -le 10 ]
	do
		echo "$count"
		sleep 1
		(( count++ ))
	done
)&
echo "Finished"

(2)后台执行或异步执行
后台执行：cmd&

(3)命令替换：'cmd' 或 $(cmd)

4、把子Shell中的变量值传回父Shell
(1)通过临时文件
(2)使用命名管道
(3)不使用子Shell

5、进程处理
(1)进程与程序的区别
程序：计算机指令的集合
进程：在自身的虚拟地址空间运行的一个单独的程序，是程序执行的基本单元。
两者区别：
<1>进程不是程序，虽然它由程序产生。
<2>程序只是一个静态的指令集合，不占系统的运行资源。而进程是一个随时都可能发生变化的、动态的、使用系统运行资源的程序。
<3>一个程序可以启动多个进程。

6、通过脚本监控进程
