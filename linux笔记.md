# 1、程序

```
1、修改文件后，要全部make clean，有的.o文件没有删除会使用原来的代码，恶心得很

2、cout 如果输出空指针，会导致程序中断，但是不会报错，要注意
3、
    String s();		这里是函数声明，不是默认构造函数创建对象
    String s;		这里是创建对象

4、类的成员函数默认都是内联函数

```

# 2、vim:

```
ctrl + l	清屏
shift + insert	Windows复制到vim
shift+k		跳转到函数定义的地方
:28,41s/A/B/g	28到41行将A修改为B
```



# 3、gdb：

```
set follow-fork-mode child			调试子进程
watch i								监视变量改变
info watchpoints					查看watch哪些变量
display i							打印i的值，每次单步进行指令后，紧接着输出被设置的表达式及值
set args 参数						   指定运行时的参数
call 函数(参数)						 调用程序中可见的函数，并传递参数，如call print(10)

x命令：			  打印内存的数据
x/nfu addr:			以 f 格式打印从 addr 地址开始的 n 个长度单元，每个单元长度为 u 的内存中值
(1)n:输出单元的个数。
(2)f:是输出格式。比如 x 是以 16 进制形式输出，o 是以 8 进制形式输出,等等。
(3)u:标明一个单元的长度。b是一个 byte，h为两个byte(halfword)，w是四个byte(word)，g是8个byte(giant word)
还可以通过x来查看主机是大端存储还是小端存储，下面这张图就是小端存储(低地址存放低字节，高地址存放高字节)
大端存储：低地址存放高字节，高地址存放低字节
```

![image-20240401185927751](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20240401185927751.png)

~~~
gdb调试多线程：
(1)查看可切换调试的线程：info threads
(2)切换调试的线程：thread 线程id
(3)只运行当前线程：set scheduler-locking on
(4)运行全部的线程：set scheduler-locking off
(5)指定某线程执行某gdb命令：thread apply 线程id gdb_cmd
(6)全部的线程执行某gdb命令：thread apply all gdb_cmd
~~~





# 4、命令：

```
tcpdump -i lo tcp port 1234 -XX -vvv -nn	抓lo口的包，并显示数据
tcpdump -i lo tcp port 1234 -nevv -w 1.pcap		抓的包保存下来

valgrind:
valgrind --tool=memcheck --leak-check=full ./a.out		检查内存泄露

ldconfig			将ld.so.conf配置文件中的所有路径下的动态库路径一次性全部缓存到ld.so.cache文件中
tail test.txt -F			动态查看文件的信息
ls -h						查看文件，内存可视化
lsb_release					查看版本号
stat file					查看文件信息

man man						查看man手册
df -h						查看磁盘
du -h						查看目录里面文件大小
du -h -d 1					查看一级目录大小
ln -s des src				创建软链接
ulimit -a					查看系统limit
ulimit -c unlimited			修改core文件大小
od -h file					显示文件16进制
ps -elf || ps -aux			查看进程
pstree -p 主线程id			  查看主线程和子线程的关系
ps -aL | grep book			查看当前运行的轻量级进程
free 						查看内存
nice						调整进程的优先级值
./main &					前台进程变成后台进程
jobs						查看本窗口的后台进程
fg 1						将后台进程拉到前台
bg 1						将后台暂停变为后台运行的状态(ctrl+z的进程)
crontab						定时任务  /etc/crontab
ipcs						查看共享内存
ipcrm						删除共享内存(ipcrm -M key)
route -n/netstat -nn 		查看路由表

nm main.o					查看符号表
ss -ltnp					查看网络连接(-l:只显示listen的socket，-t:tcp,-n:数字方式显示，-p：显示进程信息)

```



# 5、mysql:

```
alter table virtual_file modify id int(4) auto_increment;	修改字段为自增
alter table virtual_file auto_increment=1;		设置自增从1开始
```



# 6、编译

```
g++：
g++ test.cc -fno-elide-constructors 	去掉编译优化，比如查看隐似转换

gcc -c test.cc -o test.o -O0 -g (-o0编译优化为0)
```

![image-20240301115133505](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20240301115133505.png)



# 7、报错

写时复制（COW）重载[]运算符的时候，需要新建一种类型CharProxy来重载=,和<<,用于判断是否是写时，再进行字符串深拷贝，报错如下：

![image-20240228220610443](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20240228220610443.png)

代码如下：

![image-20240228220630566](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20240228220630566.png)

![image-20240228220643997](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20240228220643997.png)

这里是_pstr[index]临时变量,右值，不能被左值引用使用