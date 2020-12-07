# 二、Linux的常用命令
## 1.文件
   1）基本命令   
   - ls：列出目录
   - cd：切换目录
   - pwd：显示目前的目录
   - mkdir：创建一个新的目录
   - rmdir：删除一个空的目录
   - cp：复制文件或目录
   - rm：移除文件或目录
   - mv：移动文件或目录，或修改文件与目录的名称

实战演练：
- ls：查看一下文件目录有哪些
- mkdir tmp：创建一个tmp目录
- ls：再查看一下
- cd tmp：进入到tmp目录中
- ls：查看tmp目录下的文件
- cd .. :回到上级目录
- ls：查看上级目录中的文件
（上级目录中有个test.txt 文件，将该文件移动到tmp目录下）
- mv test.txt ~/tmp:移动到tmp文件夹中
- cd tmp:移动到tmp文件夹中
- ls：查看tmp文件夹中的文件
（可以查看到 test.txt文件）
- rm -rf test.txt: 删除test文件
- cd .. :返回上级目录
- rmdir tmp：删除tmp目录（空）
- ls -l：查看文件属性
- ls -ld：查看指定文件的属性

2）文件属性： r:读    w:写     x:执行
d r w x r - x r -x
- d:文件类型
- rwx：拥有者（可读可写可执行）
- r-x：所属组 (可读和可执行的权限)
- r-x：其他人（可读和可执行的权限）

- r:读权限 read ：4
- w：写的权限 Wright ：2
- x操作权限 execute：1

- chmod 777 test：给所有人对test文件增加 所有权限

## 2.网络
1）ping 
- -c ping：ping 的次数
- -l ：每次ping的时间间隔

2）netstat：打印Linux网络系统的状态信息
- -t：列出所有tcp
- -u：列出所有udp
- -l：只显示监听端口
- -n：以数字形式显示地址和端口号
- -p：显示进程的pid和名字
- natstat -ntlp: 所有网络信息全部显示
（如果没有这个命令，可以进行安装 yum install net-tools）

## 3.性能
1）top：持续监视系统性能（实时监控）

2）ps ：查看进程信息（快照）
- -aux：显示所有进程，包括用户、分组情况
