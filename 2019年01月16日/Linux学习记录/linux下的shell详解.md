### 10.1 认识BASH
#### 10.1.1 硬件核心与shell
简单来说就是我们是通过shell与核心进行沟通的。然后核心再和硬件进行沟通的。
#### 10.1.2 为什么学shell
因为重要。
#### 10.1.3 shell的种类
多了去了。最原始的Bourne SHell（sh），还有别的一堆，这里主要用“Bourne Again shell” 简称为`bash`，也就是目前用的。

> echo $SHELL

可以显示当前正在使用的shell

> cat /etc/shells

查看目前支持的所有的shell

> cat /etc/passwd

查看每次当你登录的时候系统分配给你的shell，我用的是zsh这个shell。

#### 10.1.4 Bash shell的功能
* 那么bash有哪些好功能呢？可以记住你输过的命令，当你退出之后会存在.bash_history中。至于记住是不是好事呢，不一定，因为黑客可以通过看历史看到一些敏感操作嘛。
* 很方便的tab补全功能。
* alias别名设置。
* 脚本语言的使用。

#### 10.1.5 type指令
`type`判断是不是shell内置指令

#### 10.1.6 指令的下达与快速编辑按钮
如果指令太长想分几行来写？用`\`即可。也就是其实是`\回车`被转义了，所以如果是`\空格回车`就失败了。

bash中可以用`ctrl+a/e`来移动到最前面或者最后面。也可以用`ctrl+u/k`来剪切光标前后的内容。

###  10.2 Shell 的变量功能
#### 10.2.1 什么是变量?
简单讲就是用一个很短的字符来替代很长的一串东西，这样会方便很多。相当于一个宏定义。

#### 10.2.2 变量的设置与显示
> echo {$PATH}

比较推荐用这种方法来显示变量，就是加上大括号。

> PATH=修改的内容

就可以修改内容。

> PATH=${PATH}:/home/usr/bin  

是对PATH进行追加。

其次是双引号内的变量还是变量，如果用单引号则变量就不在是变量，而是一个简单的值了。

目前变量对我最大的好处可能就是要进去一个比较长的路径的时候用这个变量进行保存了。

#### 10.2.3 环境变量的功能
> env 查看环境变量

* `HOME`查看相关的家目录的配置
* `SHELL` 查看目前在使用的shell是什么
* `HISTSIZE` 历史记录的个数。MAC要用`set`查看
* `MAIL` 邮件文件，但是MAC也没有
* `PATH` 可执行文件的存放位置。
* `LANG` 语言信息
* `RANDOM` 生成随机数，MAC要用`set`查看

> set 查看所有变量（包含环境变量与自定义变量）

* `COLUMNS=80`字段长度
* `HISTFILE=/Users/chenlangping/.zsh_history`历史命令记录的存放文件....怪不得我找不到
* `HISTSIZE=50000`最大内存中存五万条
* `LINES=25`一页显示25行
* `MACHTYPE=x86_64`64位机器。
* `PS1='%{%f%b%k%}$(build_prompt) '`这个是用来显示每次都在命令行最前面的那串东西的，所以是可以改的。

linux其实还可以通过`bash`指令去启动一个新的bash，这个bash会继承环境变量，然而并不会继承自定义变量，如果你希望它继承，那么可以使用`export`指令。

#### 10.2.4 乱码问题
> locale -a 查看linux支持的所有的编码

注意是locale不是locate.....
只要设置了LANG和LC_ALL，其余的会根据这两个进行自动设置。

####  10.2.5 变量的有效范围
显然自定义的变量是局部变量，而环境变量是全局变量。而export能够将局部变脸设置成全局变量。

#### 10.2.6 变量的键盘操作
主要就是通过用户的输入来决定变量。
> read [-pt] variable

p是可以接提示字符，t是等待几秒。

假设有指令`read test`，就会让用户输入，然后用户输入什么，这个`test`的环境变量的值就是什么。

> declare [-aixr] variable

a是设置成array，i是设置成整数，x的话就是和export一样了，r就是把后面的设置成只读变量。如果不小心设置了只读，那么需要退出再登录才可以恢复。

这里比较重要的当然是数组啦，这样就可以很方便的通过下标来读取啦。例如`var[1]="1"`这样子的

#### 10.2.7限制使用者的资源
因为linux是多用户多任务的操作系统，所以我们必须控制好每个用户的系统资源，不然系统容易爆掉。
这时候就需要`ulimit`来限制资源啦。
> limit [-SHacdfltu] [配额]

H是死活不让你超，S是可以超，但是超了会警告。a自然就是列出所有的限额啦，f是可以创建的最大文件大小，单位是KB。d是可以使用的最大断裂内存容量，l是可用于锁定的内存量，t是最大的CPU使用时间，u是最大的进程数。其实你直接用`ulimit -a`来查看，这些都会告诉你的

#### 10.2.8 变量的替换和删除
我觉得这部分没什么好记的，因为你直接用新的自己编辑好，然后直接替换一下就好了

### 10.3 命令别名与历史命令
#### 10.3.1 别名的设置
> alias lm='ls -al | more' 就可以让lm等价于ls -al |more了

> unalias lm 拿掉这条名字。

#### 10.3.2 历史命令:history
可以通过指定数字来查看最近的n条指令，Mac下面是`history -5`。
c选项是删除所有的history，a是把历史写入到文件中，r是读取文件到内存中，w是把内存的历史写入到文件中。需要注意的是最多只能保存的历史命令的条数取决于`HISTSIZE`这个环境变量的值。如果超了就会删除旧的去添加新的。`history`可以使用`!`来执行相关的命令，比如`!1`执行第一条，`!his`执行最近的且开头是his的命令，`!!`执行上一条，当然这个不常用，因为我们一般是按↑在回车的。
最后是如何清空历史记录呢？`history -c;history -w`就行了。

### 10.4 Bash的操作环境
#### 10.4.1 命令被执行的顺序
如果用相对/绝对路径进行了指定，那肯定是相应的路径啦，其次是去找alias，再来是使用内置的指令，找不到才会去找PATH环境变量下面的，如果连PATH都找不到的话，那就告知没有了喽。`type -a echo`就可以看到echo执行的优先级喽。

#### 10.4.2 bash的欢迎信息
登录bash显示的欢迎信息，然而我的Mac好像没有，而且还不让创建！
可以通过`cat /etc/issue`来查看，如果是通过远程登录的话，就会显示`/etc/issue.net`啦。如果是希望登录后的信息，就写`/etc/motd`啦。

#### 10.4.3 bash 的环境配置文件
就跟正常的linux一样，一些配置如果没有写入配置文件，那你只能在本次使用bash的时候生效啦。所以需要通过修改配置文件来使它永久生效。

首先是需要知道什么是login shell和nonlogin shell。login就是需要你输入密码的，而non-login比如就是用bash再去新开一个bash一样，你并没有输入密码。为什么会这样呢？

首先需要知道在login shell会读取哪些文件呢？其实只有两个：
* /etc/profile
* ~/.bash_profile 或 ~/.bash_login 或 ~/.profile

第一个还是别瞎动，容易出事。/etc/profile就是对所有使用者都生效的一个配置文件啦。
这个文件里面有一些基础设置内容。还会去读入一些其他的文件来设置，知道这些基本就差不多啦。

第二个是~/.bash_profile。在之前bash会先去读/etc/profile，读完了之后会去读：

1. ~/.bash_profile 
2. ~/.bash_login 
3. ~/.profile

但是如果第一个存在，那后面两个其实是不会去读的。
MAC这部分跟centos有些许不同，它似乎是在/etc/profile这部分就做了修改。

更改完了配置，但是如果希望它立即生效，一种办法是重启，另一种就是用`source`指令，source和`.`一个小数点都是相同的。

nonlogin-shell读的就简单多了，仅仅读取`~/.bashrc`这个文件，然而Mac还是没有.....这个主要是规范一下`umask`和`PS1`，

还有一些其他的配置，如`/etc/man_db.conf`，当你自己装软件，而且该软件带说明文档时候，可以修改这个文件指向你自己安装的文件，就可以使用`man`了。`~/.bash_history`是历史相关，`~/.bash_logout`是当你登出的时候采取的措施。

#### 10.4.4 终端机的环境设置
可以通过`stty -a`来查看所有的按键内容。`intr = ^C`就是说中断当前的程序是ctrl+c，其它的看英文应该也能看懂。如果希望设置删除是ctrl+h，那就可以`stty erase ^h`。还有一点，win下面的保存都是ctrl+s，然而这在linux中却是stop的意思，所以需要通过ctrl+q来启动。

当然还可以通过set来查看所有的变量，如果你进行了设置，可以通过`echo $-`来查看所有的设置值，我的Mac上面是一个奇奇怪怪的值。

#### 10.4.5 通配符
* *代表0到无穷多个
* ？表示必须有一个
* []中括号表示一定要有一个任意的，比如[abc]，则必须要有a或者b或者c
* [0-9]表示0到9的所有数字中的任意一个
* [^]非的意思，[^abc]是只要不是abc中的任意一个都行。

通配符在找文件的时候就很好用啦。
比如找后缀是.gz的文件就可以*.gz来查看，找文件名含有数字的文件名，就可以`*[0-9]*`来查看。

除了通配符，bash下还有一些特殊符号，比如单引号是不具备改变的功能，而双引号是有变量置换的改变的，小括号中的是子shell的命令等。

### 10.5 数据流重导向
就是你不希望它显示在屏幕上，而是希望它"显示到文件中"。就需要重导向。

#### 10.5.1 数据流重导向
首先得了解下标准输出和标准错误输出。标准输出就是执行完之后把数据显示到屏幕上，标准错误输出是发生了错误把错误的数据显示到屏幕上。标准输入是使用`<`或者`<<`，标准输出是使用`>`或者`>>`，就跟C++一样。标准错误输出是`2>`或者`2>>`，但是用的不太多。

所以重导向其实很简单，如果你不希望输出在屏幕上，那就用`>`重定向输出到文件中就行了。
`ll > ~/test/redict`其中ll的信息就会到文件中去了。注意重定向的文件如果没有会创建，如果有的话默认就是覆盖掉了。如果是对已经存在的文件，就要使用`>>`，这个并不会创建文件，而是会往已经存在的文件中追加。

`ls /test 2> ~/test/wrong` 是把错误的信息重定向到~/test/wrong文件中，而这里的/test是不存在的。，还有一种操作是，把正确的导向到a文件，错误的导向到b文件。`ls /etc >> ~/test/right 2>> ~/test/wrong`这样子，正确的话就会到right中，错误的信息就会到wrong中。还有一个神奇的文件，`/dev/null`这个文件，可以把所有导向到它的文件全部吃掉。如果想把正确的还是错误的信息都写入到同一个文件中呢？可以直接写`> all 2>&1`或者`&>all`都是不论是正确还是错误，都重导向到all这个文件中。当然有`2>&1`自然也有`1>&2`，这个的意思就是不论什么，都是错误信息显示。

`cat`单纯的这个指令是让键盘输入，然后输出到屏幕上，所以如果我用`cat > ~/test/catfile`就会把键盘中输入的导向到catfile中。注意要用ctrl+d来退出。
然后可以使用`cat > ~/test/catfile < ~/test/right`就会把right文件中的文件输入到catfile中了。那么`<<`呢？这个是指输入的结束符号，一般都是`eof`吧。就是输入到这个的时候会停止，而且这个符号本身并不会显示。

#### 10.5.2 命令执行
可以用分号来连续执行几个指令，注意的是，一定是前面的执行完来接着执行后面的。

而用`&&`和`||`可以用来判断错误。`cmd1 && cmd2`的意思就是cmd1正确执行才执行cmd2，否则就不执行cmd2。而`||`跟编程语言就有点不一样了，如果cmd1正确则cmd2不执行，如果cmd1错误则cmd2执行。这个最大的好处就是可以创建文件夹，如果第一个命令是进入这个文件夹，那么就可以`|| mkdir xxx`来确保文件夹一定会存在。所以一般的操作是`command1 && command2 || command3`。

### 10.6 管线命令
管线命令就只能处理标准输出啦（也就是输出到屏幕上的信息，如果你希望管线能够处理标准错误输出，就必须需要`2>&1`了），所以在每个管线后面的第一个肯定是指令，而且这个指令必须能够接受标准输入的指令。

#### 10.6.1 撷取命令
第一个是cut指令。cut中的 -d有点像Java的split方法，利用特殊的分割字符把原字符串切成数组，然后用下标来访问，但是下标是从1开始的。`cut -d'分隔字符' -f field`，比如找出PATH的第二项，就可以`echo ${PATH} | cut -d ':' -f 2`。当然也可以取出每一行的固定范围啦，就是用-c，比如我想看环境变量的第2列后面的，就可以`dev | cut -c 2-`啦。

第二个就是最常用的grep啦。grep中用的比较多的选项是-c计算出现的次数、-i忽略大小写、-n输出行号和-v反向选择。还有`--color=auto`来显示颜色，不过这个已经在alias中了。

#### 10.6.2 排序命令
第一个是sort指令。一些重要的选项：
* -f 忽略大小写
* -r 反向排序
* -t 指定分隔符
* -k 以该区间进行排序
* -n 以数字进行排序，默认是文字哦

第二个是uniq指令。就是有些时候有些数据重复而又不需要，就可以用这个啦。
* -i 忽略大小写
* -c 进行计数

最后一个是我艹指令！应该是word count=wc，就是word里面的字数统计。如果不指定则默认下面三个都进行输出：
* -l 仅仅列出行
* -w 列出有多少字
* -m 列出多少字符

#### 10.6.3 双向重导向
有一个问题就是我希望把数据一份保存到屏幕上而且这一份数据也存到文件中，之前的重导向就不好使了，就需要双向重导向啦。很简单的理解就是它把你的数据流复制了一份，这样两份就可以继续都处理了。
用的是`tee`指令。注意tee是默认会放一份到屏幕上的，所以你只要写明另一条是到文件中还是继续传递就行了。选项只有一个是-a，用累加的方法来输入到文件中。举例`last | tee last.list | 接着处理`

#### 10.6.4 字符转换命令
一共五个指令。
第一个是`tr`，有两个选项：
* -d 删除
* -s 取代掉重复的

默认是取代！别忘记了！所以直接`tr a A`就可以把所有的小写a转换成大写的A。而`tr '[a-z]' '[A-Z]'`则是把所有的小写都转换成大写。`tr -d '\r'`删除所有的`^M`.

第二个是`col`，似乎就是用来把tab转换成空白键....
`col -x`即可。

第三个是join。简单来说就是把两行合成一行，但是这两行确实是有相同的内容的。
`join [-ti12] file1 file2`
* -t 是用来指定分隔符，不指定就是空格吧
* -i 忽略大小写
* -1 第一个文件用哪个来分析
* -2 第二个文件用哪个来分析

这个说实话蛮难的，它会把相同的拿出来先合成一个放到最前面还有再做处理，有这个指令的印象就行了。

第四个是`paste`，这个就是简单粗暴把两行捏在一起，中间加个指定的分隔符，用-d来指定分隔符。默认是tab作为分隔符的。

第五个是`expand` 直接把tab转成空格。

#### 10.6.5 分区命令
就是分割文件的。
`split [-bl] file PREFIX`
* -b 指定每个文件的大小 5m这种的
* -l 以行数进行分区
* PREFIX 这个就有点意思了，因为它可能会把你的文件分成四份，那第一份就叫PREFIXaa这样子的。

#### 10.6.6 参数代换
这部分的主要指令是`xargs`，就是其实很多指令并不支持pipe，这时候就要在这些指令前面加上这个，这个相当于每次都会把参数给这些指令，这样子似乎也就像这些指令支持了。

#### 10.6.7 关于减号的用途
减号就是标准输入输出。
