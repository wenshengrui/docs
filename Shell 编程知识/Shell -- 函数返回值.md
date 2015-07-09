	Linux Shell脚本编程－－函数返回值

Shell函数返回值，常用的两种方式：return，echo
1） return 语句
shell函数的返回值，可以和其他语言的返回值一样，通过return语句返回。
示例：
[plain] view plaincopy在CODE上查看代码片派生到我的代码片
#!/bin/sh    
function test()    
{    
    echo "arg1 = $1"    
    if [ $1 = "1" ] ;then    
        return 1    
    else    
        return 0    
    fi    
}    
    
echo     
echo "test 1"    
test 1    
echo $?         # print return result    
    
echo     
echo "test 0"    
test 0    
echo $?         # print return result    
    
echo     
echo "test 2"    
test 2    
echo $?         # print return result    

输出结果为：
test 1
arg1 = 1
1
－－－－－－－－－－－－－－－－－－－－－
test 0
arg1 = 0
0
－－－－－－－－－－－－－－－－－－－－－
test 2
arg1 = 2
0
－－－－－－－－－－－－－－－－－－－－－
先定义了一个函数test，根据它输入的参数是否为1来return 1或者return 0。
获取函数的返回值通过调用函数，或者最后执行的值获得。
另外，可以直接用函数的返回值用作if的判断。
注意：return只能用来返回整数值，且和c的区别是返回为正确，其他的值为错误。
 
3） echo 返回值
其实在shell中，函数的返回值有一个非常安全的返回方式，即通过输出到标准输出返回。因为子进程会继承父进程的标准输出，因此，子进程的输出也就直接反应到父进程。
示例：
[plain] view plaincopy在CODE上查看代码片派生到我的代码片
#!/bin/sh    
function test()    
{    
    echo "arg1 = $1"    
    if [ $1 = "1" ] ;then    
        echo "1"    
    else    
        echo "0"    
    fi    
}    
    
echo     
echo "test 1"    
test 1    
  
    
echo     
echo "test 0"    
test 0    
  
    
echo     
echo "test 2"    
test 2    
 
结果：
test 1
arg1 = 1
1
－－－－－－－－－－－－－－－－－－－－－
test 0
arg1 = 0
0
－－－－－－－－－－－－－－－－－－－－－
test 2
arg1 = 2
0
 
怎么？有没有搞错，这两个函数几乎没什么区别，对，它几乎就是return和echo不一样，但是有一点一定要注意，return不能向标准输出一些不是结果的东西（也就是说，不能随便echo一些不需要的信息），比如调试信息，这些信息可以重定向到一个文件中解决，特别要注意的是，脚本中用到其它类似grep这样的命令的时候，一定要记得1>/dev/null 2>&1来空这些输出信息输出到空设备，避免这些命令的输出。