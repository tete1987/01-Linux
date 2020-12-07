# 一、Linux基本使用方法
## 1.常用的shell：
- Bourne Shell（/user/bin/sh或/bin/sh）
- Bourne Again Shell (/bin/bash)
- C Shell (/user/bin/csh)
- K Shell (/user/bin/ksh)
- Shell for Root(/sbin/sh)
## 2. bash 示例：
  ```
    #!/bin/bash'
    echo "hello!" 
  ```

执行该文件：chmod +x ./tesh.sh
./test.sh  执行脚本

或：
   /bin/sh test.sh  
   指定了Bourne shell 
   执行（与Bourne Again shell 区别很小）
   
## 3.实战演练：
- ls：查看文件目录
- rm -rf  test.sh :删除test.sh文件
- vim test.sh:新建test.sh文件的同时进行编辑
    ```
   #！/bin/bash
    echo "hello!"
    
     :wq(保存加退出)
   ```
        
- cat test.sh: 查看test文件的内容
- ./test.sh :执行该文件#
- chmod +x test.sh:修改test文件的权限，增加可执行的权限
- /bin/sh test.sh :执行该文件

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

# 三、Linux三剑客与管道使用
## 1.管道：
Linux提供管道符“|”将两个命令隔开，管道符左边命令的输出就会作为管道符右边命令的输入。

示例：
`echo "hello 12345"|grep  "hello"    `

## 2.正则表达式:
正则表达式就是记录文本规则的代码。
演练环境：https://tool.oschina.net/regex

举例：
- 1）找出所有的 hi单词：\bhi\b
- 2）hi 单词后面有lucy 单词：\bhi\b.*\blucy\b
- 3）以0开头，然后是两个数字，然后是一个连字号“-” ，最后是8个数字：0\d{2}-\d{8}

正则语法：
|代码|描述|
|---:|---:|
|. |匹配除换行符以外的任意字符|
|\w |匹配字母或数字或下划线或汉字|
|\s |匹配任意的空白符|
|\d |匹配数字|
|\b |匹配单词的开始或结束|
|^  |匹配字符串的开始|
|$  |匹配字符串的结束|
|*  |重复零次或更多次|
|+ |重复一次或更多次|
|？  |重复零次或一次|
|{n} |重复n次|
|{n,}  |重复n次或更多次|
|{n,m}  |重复n到m次|

实战：
- 1）匹配以a字母开头的单词：\ba\w*\b
- 2）匹配刚好6个字符的单词:\b\w{6}\b
- 3）匹配1个或更多连续的数字:\d+
- 4）5到12位QQ号码：\d{5,12}  （这种方法有个问题，如果前面后面都有字符，依然可以匹配，可以使用：^\d{5,12}$   来去掉带有字符的数字)   

3.grep：根据用户指定的模式（pattern）对目标文本进行过滤，显示被模式匹配到的行。
命令形式：grep[OPTION]PATTERN[FILE……]

选项：
1) -v：显示不被pattern匹配到的行
2) -i：忽略字符大小写
3) -n：显示匹配的行号
4) -c：统计匹配的行数
5) -o：仅显示匹配到的字符串
6) -E：使用ERE，相当于egrep

实战1：
- 1）查找文件内容包含root的行数：grep -n root test.txt
- 2）查找文件内容不包含root的行：grep -nv root test.txt
- 3）查找以s开头的行：grep  ^s test.txt
- 4）查找以n结尾的行：grep n$ test.txt

## 4.sed：
sed是流编辑器，一次处理一行内容。

清空模式空间—>行存储在模式空间—>sed命令处理—>送入屏幕—>（循环到）清空模式空间
命令形式：sed[-hn..][-e<script>][-f<script FILE>][FILE]
命令解析：
- 1）[-hn]
      -h: 显示帮助
      -n：仅显示script处理后的结果
- 2）[-e<script>][-f<script FILE>]
     -e<script>：以选项中指定的script来处理输入的文本文件。
     -f<script文件>：以选项中指定的script文件来处理输入的文本文件。 

常用动作：
- 1）a：新增  sed -e '4 a newline'(使用sed执行一个脚本，脚本的内容是：在第四行新增加一个 newline)
- 2）c：取代  sed -e '2,5c No 2-5 number'（使用sed执行一个脚本，脚本的内容是使用“No 2-5 number”来取代2到5行的内容）
- 3）d：删除  sed -e '2,5d'（使用sed执行一个脚本，脚本的内容是删除2到5行）
- 4）i：插入   sed -e '2i newline'(使用sed执行一个脚本，脚本的内容是：在第二行前面插入一个新行，叫： newline)
- 5）p: 打印    sed -n '/root/p'（打印匹配到“root”的内容）
- 6）s：取代   sed -e 's/old/new/g'（使用后面的内容new取代前面的old，/g 是代表全局的意思）

实战1：
1）查看帮助：
      `man  sed
      sed -h`
2）在第四行后天就新的字符串：
      ` sed  '4 a neline testfile' test.txt`
3）在第二行后加上newline
      `sed ‘2a drink tea’ test.txt`
4）在第二行前加上newline
     ` sed '2i drink tea' test.txt`
5）全局替换
      `sed -e ‘s/root/hello/g’ test.txt`
6）直接修改文件内容
     ` sed -i 's/root/hello/g' test.txt`
     
## 5.awk:
定义：把文件逐行的读入，以空格为默认分隔符将每行切片，切开的部分在进行后续处理。

把行作为输入，并赋值给$0—>将行切段，从$1开始—>对行匹配正则/执行动作—>打印内容—>（循环到）把行作为输入，并赋值给$0

命令形式：awk 'pattern+action'[FILE]

命令解析：
- 1）pattern+action
     - -pattern 正则表达式
     - -action 对匹配到的内容执行的命令（默认为输出每行内容）

常用参数：
- 1） FILENEME:awk 浏览的文件名
- 2） BEGIN：处理文本之前要执行的操作
- 3） END：处理文本之后要执行的操作
- 4） FS：设置输入域分隔符，等价于命令行 -F选项
- 5） NF：浏览记录的域的个数（列数）
- 6） NR：已读的记录数（行数）
- 7） OFS:输出域分隔符
- 8） ORS:输出记录分隔符
- 9） RS:控制记录分隔符
- 10） $0:整条记录
- 11）$1:表示当前行的第一个域……以此类推

实战1：

1）搜索/etc/passwd 有root 关键字的所有行，并显示对应的shell
      
    `awk -F：‘/root/{print $7}’ /etc/passwd`

2）打印/etc/passwd/的第二行信息

    `awk -F：'NR==2{print $0}' /etc/passwd`

实战2：
1）使用begin加入标题

`awk 'BEGIN {print "BEGIN","BEGIN"}{print $1 $2}' /etc/passwd `

2）自定义分隔符

`ech "111 222|333 444|555 666"|awk ‘BEGIN{NR=“|”}{print  $0}’`

