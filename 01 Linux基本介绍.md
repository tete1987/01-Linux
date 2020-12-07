#一、Linux基本使用方法
##1.常用的shell：
- Bourne Shell（/user/bin/sh或/bin/sh）
- Bourne Again Shell (/bin/bash)
- C Shell (/user/bin/csh)
- K Shell (/user/bin/ksh)
- Shell for Root(/sbin/sh)
##2. bash 示例：
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
   
##3.实战演练：
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
