### 前言
为什么要装中文的man命令呢？因为英文的man命令看上去累，而且两者并不冲突。想快速浏览的时候就用中文的man，想仔细看的时候就用man
> 参考资料：https://blog.csdn.net/FungLeo/article/details/78522691

### 安装
首先安装编译工具
```
brew install automake
brew install opencc
```

然后把项目下载下来：
`git clone https://github.com/man-pages-zh/manpages-zh.git`

进入文件夹，进行编译并且安装：
```
cd manpages-zh
autoreconf --install --force
./configure
make
sudo make install
```

接下来的操作要取决于你用的bash了：
1. 如果是自带的：
`echo "alias cman='man -M /usr/local/share/man/zh_CN'" >> ~/.bash_profile`
`source ~/.bash_profile`
2. 我用的是zsh：
`echo "alias cman='man -M /usr/local/share/man/zh_CN'" >> ~/.zshrc`
`source ~/.zshrc`

此时可以用`cman ls`查看下是不是中文，如果是的话就可以了。如果出现乱码则需要继续。

### 安装 groff解决乱码问题
下载源码包
`wget http://git.savannah.gnu.org/cgit/groff.git/snapshot/groff-1.22.tar.gz`

解压缩：
`tar zxvf v1.6.3.2.tar.gz`

进入文件夹并且进行编译安装
```
cd groff-1.22
./configure
sudo make
sudo make install
```
最后在编辑一个文件即可:
`sudo vim /etc/man.conf` 在这个文件的最后加入:`NROFF preconv -e UTF8 | /usr/local/bin/nroff -Tutf8 -mandoc -c` 然后`cman ls`就可以正常显示了
