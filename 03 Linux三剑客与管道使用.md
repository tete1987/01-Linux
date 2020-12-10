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
|---|---|
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
