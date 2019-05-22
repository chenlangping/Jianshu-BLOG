### 12.1 什么是 Shell scripts
简单讲就是你把一些日常做的复杂的工作写个程序来解放你啦。

#### 12.1.1 为什么要学习？
因为重要。

#### 12.1.2 第一个script
1. 从上到下，从左到右应该不用说了吧
2. 读到回车会执行命令，所以要用转义符+回车来转移。
3. #后面的内容是注释

那么如何执行呢？首先必须要有可读可执行的权限，，其次可以用相对/绝对路径来执行，最后还可以放到PATH下面直接执行。其实还可以用bash/sh这两个指令来执行啦，但是如果用bash的话，只需要可读就行啦。

```
#!/bin/bash
echo -e "Hello World! \a \n"
exit 0
```
标准的helloworld就是这样了。第一行宣告用的是bash的语法，程序运行的时候会载入bash相关的配置。然后就是党营了。这里最后还有一个执行结束的返回值，很好的例子。最后，其实还可以在自己的程序内写好程序的环境变量。

### 12.2 简单的例子
1. 有用户输入的例子
```
#!/bin/bash
# Program:
# let user input his/her first name and last name. Show his/her full name
# History:
# 2018年12月01日 champion

PATH=/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin
export PATH

read -p "input your first name:" firstname
read -p "input your last name:" lastname
echo -e "your full name is ${firstname} ${lastname}"
```

2. 利用日期变化来创建文件
利用date即可啦。
```
#!/bin/bash
# Program:
# create file by user's input and date
# History:
# 2018年12月01日 champion

PATH=/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin
export PATH

read -p "input file name which you want to create:" filename

# create date
date1=$(date --date='2 days ago' +%Y%m%d)
date2=$(date --date='1 days ago' +%Y%m%d)
date3=$(date +%Y%m%d)

# create three file name
file1=${filename}${date1}
file2=${filename}${date2}
file3=${filename}${date3}

# touch file
touch "${file1}"
touch "${file2}"
touch "${file3}"
```
