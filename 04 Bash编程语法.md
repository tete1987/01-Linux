# 四、Bash编程语法
1.变量

1）规则：
  - 命名只能使用英文字母，数字和下划线，首字符不能以数字开头
  - 中间不能有空格，可以使用下划线
  - 不能使用标点符号
  - 不能使用bash里的关键字（可用help命令查看保留关键字）
  
2）定义与使用变量
- your_name="abc"
- necho $your_name(打印变量)

3）只读变量
```
a=“123”
readonly  a
```
4）删除变量

`unset variable_name`

（不能删除只读变量）

5）变量类型
- 字符串：your_name="tete"
- 拼接字符串：greeting=“hello，“$your_name”!”
- 数组：array_name=(value0 value1 value2 value3)
- 取数组：valuen=${array_name[n]}
- 单独赋值：array_name[0]=value0

实战1：
```
1.使用变量
   a="abc"
   echo $a
  
2.删除变量
   unset  a
```

实战2：
```
1.数组初始化
   my_array=(A  B "C" D)
   echo “第一个元素为：${my_array[0]}”
   
2.数组单个定义
   my_array[1]=B
   echo "数组的元素为：${my_array[*]}"
   echo "数组的元素为：${my_array[@]}"

2.控制语句
   1）条件分支：if
         定义：
          if condition
          then  
             command1
             command2
             ……
             commandN
           fi
示例：
if [ 2==2 ]; then echo "true";else echo "false";fi
if[ 2> 1 ]; then echo "true";else echo "false";fi

           -gt:大于    -lt：小于
  ```
  
  实战：
  ```
1）比较两个变量的大小并输出不同的值

if [$a -eq $b ]; then echo "equal"; elif [$a -lt $b]; then echo "small"; elif [$a -gt $b]; then echo "big" ; fi
```
 2）循环：for
    
    定义：
    for var in item1 item2 ……itemN
    do
      command1
      command2
      ……
      command N 
    done
```
示例：
    for loop in 1 2 3 4 5 
    do 
        echo "hello"
    done
```
实战：
```
1.循环读取文件内容并输入： for i in $(cat dir.txt);do echo $i;done
```
3）循环：while
             定义：
             while condition
             do 
                   command
              done
```
示例：
         int=1
         while(( $int<=5 ))
         do
            echo $int
            let "int ++"
          done
```

实战：
1.循环读取文件内容并输出：

`while read line;do echo $line;done<dir.txt`

