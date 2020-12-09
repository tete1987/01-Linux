# 八、Linux三剑客实战
## 1.awk
1）搜索

/etc/passwd 有root 关键字的所有行，并显示对应的shell

    awk -F：‘/root/{print $7}’ /etc/passwd`

注解：-F 指定分隔符

2）打印/etc/passwd/的第二行信息

    awk -F：'NR==2{print $0}' /etc/passwd

3）使用begin加入标题

    awk 'BEGIN {print "BEGIN"，"BEGIN"}{print $1,$2}' /etc/passwd
4)自定义分隔符

     echo “111 222|333 444|555 666” | awk 'BEGIN{RS="|"}{print $0}'

5）实战练习：

①
```
echo "aaaaac
bbbbbdd
kkkkkf
vvvvva" | awk '/aaaa/{print}'
```
结果：aaaaac


②
```
echo "aaaaac|bbbbb|cccc|kkkkkf|vvvvva" | awk -F '|' '{print $2}'
```
结果：bbbbb（以 | 为分隔符，打印第二列）

③
```
echo "aaaaac
bbbbbdd
kkkkkf
vvvvva" |wc -l
```
注：wc -l  返回个数。
结果：4

④
```
echo "aaaaad
bbb|eee
333|ttt
ffff|ggg|uuuu"  | awk -F '|' 'NR!=1{print}' K >tmp.txt
```
查看tmp.txt的结果是：
```
bbb|eee
333|ttt
ffff|ggg|uuuu
```

⑤ 也可以使用命令加入到awk中，但不常用，如下：
```
echo "sssss
cccc
dddd
eeee" | awk '{cmd="echo cccc";system=(cmd)}'
```

⑥ 查看Nginx日志中的404 和500的报条数

    grep -E '404 | 500 ' nginx.log |wc -l

结果：显示404 和500 的行数

使用awk可使用下列写法：
`awk '$9~/404/500' nginx.log |wc -l`
   

注：$9 是日志中的第九列，~是print的简写，意思是只要遇到符合条件的就打印。awk中必须使用/ 才能标志这是正则。
结果相同。

2.找出日志中访问量最高的IP：

解题思路：

1）先筛选出日志中IP的列数：
`awk '{print $1}' nginx.log`

2）再对IP地址进行排序(sort)：
`awk '{print $1}' nginx.log |sort`

3）合并相同的IP地址(uniq -c)：
`awk '{print $1}' nginx.log |sort |uniq -c `

4）对结果进行倒序排序：
`awk '{print $1}' nginx.log |sort |uniq -c|sort -nr`

5）找出前3(head)：
`awk '{print $1}' nginx.log | sort |uniq -c |sort -nr|head 3`


6)使用grep 进行找出日志中访问量最高的IP

`grep -Eo '^[0-9]*.[0-9]*.[0-9]*.[0-9]*' nginx.log |sort |uniq -c |sort -nr |head 3`

7) 将上面的写法优化一下：

`grep -Eo '^([0-9]*.){3}[0-9]*' nginx.log |sort|uniq -c | sort -nr|head 3`

8)因为 . 在正则里会匹配所有，所有需要转义一下：

`grep -Eo '^([0-9]*\.){3}[0-9]*' nginx.log |sort|uniq -c |sort -nr|head 3`


3.将topics 后面的数字替换成number

`sed 's@topics/[0-9]*@topics/numbers@g' nginx.log`

注：@可换成任意字符

4.将IP地址横向打印

1）首先筛选IP地址：
`awk '{print $1}' nginx.log`

结果：每个IP地址都是一行，显示出来

2)将后面的行追加到前面来：
`awk '{prinrt $1}' nginx.log |sed 'N;s/\n/|/g'`

注：N：把下一行追加到此行；   将  \n（换行符） 替换为 | 

3）再加标记：
`awk '{print $1}' nginx.log | sed ':1;N;s/\n/|/g;t1'`

注：1可以变为任意字符，只要与t后面的数字一致即可，意为每次跳转的位置。
