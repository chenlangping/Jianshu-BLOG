&#160; &#160; &#160; &#160;以前一直使用ssh(secure shell)远程登陆腾讯云的服务器，因为一直使用Windows，所以一直用的Xshell，不过说实话挺好用的，直接双击就能连接上服务器使用了。

&#160; &#160; &#160; &#160;最近换了Mac，觉得Mac上面的**iTerm2**也挺好用的，就想到用这个当做我的shell了。就跑去学习了ssh的具体使用方法。结果是自己不小心把服务器的密码忘记了，因为一直在使用Xshell嘛……于是去重置了密码，结果不知道什么原因还是死活登不上，然后去问了同学，他说可以使用公私钥的配置方法方便登陆，于是就有了接下来的记录。
> 参考资料：http://www.ruanyifeng.com/blog/2011/12/ssh_remote_login.html

`SSH（Secure Shell）`是一个提供数据通信安全、远程登录、远程指令执行等功能的安全网络协议，由芬兰赫尔辛基大学研究员`Tatu Ylönen`，于1995年提出，其目的是用于替代非安全的Telnet、rsh、rexec等远程Shell协议。之后SSH发展了两个大版本SSH-1和SSH-2。

通过使用SSH，你可以把所有传输的数据进行加密，这样"中间人"这种攻击方式就不可能实现了，而且也能够防止 `DNS欺骗`和 `IP欺骗`。使用SSH，还有一个额外的好处就是传输的数据是经过压缩的，所以可以加快传输的速度。SSH有很多功能，它既可以代替Telnet，又可以为FTP、Pop、甚至为PPP提供一个安全的"通道"。

SSH为一项创建在**应用层**和**传输层**基础上的安全协议，为计算机上的 Shell 提供安全的传输和使用环境。

&#160; &#160; &#160; &#160;首先是生成公私钥：`ssh-keygen`  会询问你公私钥存放的地址，然后配置一个passphrase，确认passphrase。我直接就在默认的地方生成了公私钥：`id_rsa`和`id_rsa.pub`
&#160; &#160; &#160; &#160;然后把公钥上传到服务器上面：`scp -P <端口号> ~/.ssh/id_rsa.pub <用户名>@<ip地址>:/home/id_rsa.pub`  然后再把公钥追加到服务器的ssh认证文件中：`cat /home/id_rsa.pub >> ~/.ssh/authorized_keys`  [如果提示没有这个文件就创建一个，创建之前需要先打开`/etc/ssh/sshd_config` ，查看一下`AuthorizedKeysFile` 对应的值，这个值就是你应该创建的目的地址，之后还可以禁止密码登录保证安全]
&#160; &#160; &#160; &#160;然后就可以用私钥进行登录了：`ssh <用户名>@<ip>`
&#160; &#160; &#160; &#160;但是这么做还是不太优雅，可以进一步配置`~/.ssh`下面的config文件（如果没有就创建），这样会更方便:
```
Host            alias            #自定义别名
HostName        hostname         #服务器的ip地址
Port            port             #ssh服务器端口，默认为22
User            user             #ssh服务器用户名
IdentityFile    ~/.ssh/id_rsa    #服务器公钥文件对应的私钥文件的路径
```
&#160; &#160; &#160; &#160;我的配置：
```
Host tencent
Hostname 118.89.**.**
User ubuntu
IdentityFile ~/.ssh/tencent
```
&#160; &#160; &#160; &#160;最后直接可以用`ssh tencent` 就可以登录服务器了。
&#160; &#160; &#160; &#160;PS：腾讯云可以直接上控制台生成公私钥，这样只需要配置下config就行了，非常方便
&#160; &#160; &#160; &#160;PPS:使用`logout`可以退出ssh链接

###遇到过的问题：
######1. ssh连接不上去
可能是ip被禁了，可能是端口被禁了(我的是这种)，还有可能是防火墙问题。ip如果被禁了，去你的vps控制面板换一个ip，如果是端口被禁了，使用`vim /etc/ssh/sshd_config`，找到`Port`，改一下，然后`service sshd restart`即可。防火墙问题，首先关闭防火墙`service iptables stop`，然后修改配置`vim /etc/sysconfig/iptables` ，改成`-A INPUT -m state --state NEW -m tcp -p tcp --dport 端口号 -j ACCEPT`，然后启动防火墙即可。

