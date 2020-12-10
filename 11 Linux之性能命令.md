# 十一、Linux之性能命令
## 一、总体态势
![linux总体态势](https://github.com/tete1987/picture_resource/blob/master/linux%E6%80%BB%E4%BD%93%E6%80%81%E5%8A%BF.png)


## 二、技术要点
### 1.安装

1）CentOs/Red Hat
`yum -y install sysstat`

2）other
`http://sebastien.godard.pagespetso-orange.fr/download.html`

### 2.uptime

`17:03:39 up 122 days,19:19,16 users,load average:0.52,1.26,0.97`

注解：

- 17:03:39  当前时间

- up 122 days  已开机122天

- 19:19  已开机多少天之外的多少个小时

- 16 users  16个用户在线

-  load average 平均负载
 
-  0.52,1.26,0.97  分别是1min 5min  15min 的平均负载值
 
- 平均负载=（可运行进程+不间断进程）/2

1)runnable：可运行进程

2)uninterruptable：不间断进程

3)1,5,15

### 3.统计有多少用户：
 cat /ect/group | wc -l

### 4.深入理解负载：
1）

- cpu==1

load average == 1, cpu 时刻在用

- cpu==4
load average ==1,cpu 只使用25%

2）平均负载：
 平均负载不大于3，则系统运行表现良好
  如果多核cpu，需要累加
                 4核 cpu<12

### 5.dmesg 
系统信息
`dmesg | tail`

显示系统信息：
![linux-dmesg](https://github.com/tete1987/picture_resource/blob/master/linux-dmesg.png)

### 6.vmstat 1
![linux-vmstat](https://github.com/tete1987/picture_resource/blob/master/linux-vmstat.png)

注解：
r:可运行进程数

b:block表示进程被cpu以外的状态给阻断了，比如是硬盘，网络，当我们进程发一个数据包，网速快很快就能发完，但是当网速太慢，就会导致b的状态

swpd：虚拟内存（交换区原理如图，图1为覆盖模式，图2为交互模式）

![linux-交换区-覆盖](https://github.com/tete1987/picture_resource/blob/master/linux-%E4%BA%A4%E6%8D%A2%E5%8C%BA-%E8%A6%86%E7%9B%96.png)

![linux-交换区-交互](https://github.com/tete1987/picture_resource/blob/master/linux-%E4%BA%A4%E6%8D%A2%E5%8C%BA-%E4%BA%A4%E4%BA%92.png)


- free：空闲内存
- buff：缓冲，速度对等缓冲，主要是视频类的（数据传输）
- cache：缓存，速度不对等的缓冲，主要是cpu（保险柜）
- si：交换区写入内存
- so：交换区读入内存
- bi：每秒读取的块数（读磁盘）
- bo：每秒写入的块数（写磁盘）
- in：中断数，分类为软中断和硬中断。
- 软中断：软件引起的中断（例如：除零异常）
- 硬中断：硬件引起的中断（I/O）
- cs：每秒上下文切换数
- us：非进程内核，用户进程执行消耗cpu时间(user time)
- sy：内核进程，系统进程消耗cpu时间(system time)
- id：空闲(包括IO等待时间)
- wa：等等IO时间（Wa过高时，说明io等待比较严重，这可能是由于磁盘大量随机访问造成的，也有可能是磁盘的带宽出现瓶颈。）

7.提高runnable 

（是最方便的提高性能的方法之一）

写一个死循环脚本，使其运行50次，  &表明后台运行

1）第一种写法：
```
#!/bin/bash
for i in {1..50};do
 {while true;do
     ((2+2))
   done  
 } &
done
```

2）第二种写法：
```
#!/bin/bash
for ((i=0;i<50;i++));do
 {while true;do
    ((2+2))
   done  
 } &
done
```

查看进程：

`ps -aux | grep test.sh | awk '{cmd ="kill -9 " $2;system(cmd)}'`


常见问题处理

如果r经常大于4，且id经常少于40，表示cpu的负荷很重。

如果pi，po长期不等于0，表示内存不足。

如果disk经常不等于0，且在b中的队列大于3，表示io性能不好。

1.)如果在processes中运行的序列(process r)是连续的大于在系统中的CPU的个数表示系统现在运行比较慢,有多数的进程等待CPU。

2.)如果r的输出数大于系统中可用CPU个数的4倍的话,则系统面临着CPU短缺的问题,或者是CPU的速率过低,系统中有多数的进程在等待CPU,造成系统中进程运行过慢。

3.)如果空闲时间(cpu id)持续为0并且系统时间(cpu sy)是用户时间的两倍(cpu us)系统则面临着CPU资源的短缺。

解决办法:
当发生以上问题的时候请先调整应用程序对CPU的占用情况。使得应用程序能够更有效的使用CPU，同时可以考虑增加更多的CPU。

关于CPU的使用情况还可以结合
`mpstat,  ps aux top  prstat –a`
等等一些相应的命令来综合考虑关于具体的CPU的使用情况，和那些进程在占用大量的CPU时间。

一般情况下，应用程序的问题会比较大一些.比如一些sql语句不合理等等都会造成这样的现象。

内存问题现象:
- 内存的瓶颈是由scan rate (sr)来决定的。
- scan rate是通过每秒的始终算法来进行页扫描的。
- 如果scan rate(sr)连续的大于每秒200页则表示可能存在内存缺陷。同样的如果page项中的pi和po这两栏表示每秒页面的调入的页数和每秒调出的页数。如果该值经常为非零值,也有可能存在内存的瓶颈,当然,如果个别的时候不为0的话，属于正常的页面调度这个是虚拟内存的主要原理。

解决办法: 

1.调节applications & servers使得对内存和cache的使用更加有效。

2.增加系统的内存。
3. 
```
Implement priority paging in s in pre solaris 8 versions by adding line "set priority paging=1" in /etc/system. Remove this line if upgrading from Solaris 7 to 8 & retaining old /etc/system file.
```

关于内存的使用情况还可以结`ps aux top  prstat –a`等等一些相应的命令来综合考虑关于具体的内存的使用情况,和那些进程在占用大量的内存。

一般情况下，如果内存的占用率比较高,但是,CPU的占用很低的时候,可以考虑是有很多的应用程序占用了内存没有释放。

但是,并没有占用CPU时间,可以考虑应用程序,对于未占用CPU时间和一些后台的程序,释放内存的占用。

### 8.iostat 1 : 块设备（磁盘）的状况

![linux-iostat](https://github.com/tete1987/picture_resource/blob/master/linux-iostat.png)

1）tps：每秒进程下发的IO读、写请求数量

2）kB_read/s:每秒从驱动器读入的数据量

3）kB_wrtn/s:每秒从驱动器写入的数据量

4）kB_read:读入数据总量

5）kB_wrtn:写入数据总量

### 9. Linux 文件
1）一切皆是文件
    `cd/dev`
    
2）设备是由udev进行管理，udev配置文件
   `/etc/udev/udev.conf`

![linux-理解iowait1](https://github.com/tete1987/picture_resource/blob/master/linux-%E7%90%86%E8%A7%A3iowait1.png)

![linux-理解iowait2](https://github.com/tete1987/picture_resource/blob/master/linux-%E7%90%86%E8%A7%A3iowait2.png)


### 10.free -m

![linux-free-m](https://github.com/tete1987/picture_resource/blob/master/linux-free-m.png)

### 11.top
-n ：获取多次CPU的执行情况，top -n 4 只更新4次

-d ：间隔时间，top -4 每隔4秒 更新一次

-p ：获取指定端口的进程的数据，top -p 4444

`for i in {1..20};do top -n |-p 1|grep systemd |awk '{print $11}'; done`

