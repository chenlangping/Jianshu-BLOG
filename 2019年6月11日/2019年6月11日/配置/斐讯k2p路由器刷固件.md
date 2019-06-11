## 前言
最近心血来潮，买了个斐讯的K2P来玩玩，这款路由器有以下优点：
- 直接配置小飞机，这样网内的所有设备都可以科学上网
- 千兆网口，但是所里这网.....百兆都完全跑不满
- 去除广告
- 其它可玩的东西

我买的k2p是银色的A2版本，固件版本是`22.7.8.5`

## 失败经历
收到之后首先去找了[这篇文章](http://www.upantool.com/sense/2017/11463.html)，根据文章指示，刷入完opboot之后始终进不去`192.168.1.1`，只好放弃。后来知道原因，因为笔记本进不去，台式机能进去....

## 成功经验
然后继续上网找资料，发现知乎有篇文章写的好像很简单：
> https://zhuanlan.zhihu.com/p/51859862  

简而言之就是两步：
1. 为路由器刷入breed Web
2. 有了breed Web之后就可以刷入任何你喜欢的固件

而为路由器刷入breed Web有傻瓜式软件，这里提供两个版本：
> 链接:https://pan.baidu.com/s/1vjdXBUAn8b-gNC5a7W6cuQ  密码:ch0u

我用的是傻瓜式软件版本是`v5.8`，我的固件是`22.7.8.5`，结果发现不可以。之后又去试了很多方法，都不行。后来仔细看了傻瓜式软件的作者的文档，说`v5.8`在`22.8.5.189`上是可以刷的，然后我就开始曲线救国：手动升级路由器的固件到`22.8.5.189`，然后再用这个软件刷！果然成功了。下面是升级到`22.8.5.189`的文件，也可以自行[官网](http://www.phicomm.com/cn/support.php/Soho/software_support/t/sm/p/2.html)下载。
> 链接:https://pan.baidu.com/s/1Stc6e95vCuDfKuQc2Nv4jg  密码:tr8h

也就说到这里第一步为路由器刷入breed Web已经成功了，然后就是到breed Web中刷入固件，具体物理操作：路由器断电，按下reset按钮**不放**，然后通电，这里持续按着reset按钮大约10秒，然后松手，这个时候路由器应该是蓝灯闪烁状态，然后将你的电脑设置成自动获取ip，然后登陆192.168.1.1，选择固件更新就可以了。
![image](http://upload-images.jianshu.io/upload_images/13050335-2bd38eabbbb1dab4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**注意！这里有一点超级超级坑！我的笔记本电脑连不上192.168.1.1，非要换成台式机才可以**

最后就是固件的提供了，这里有一个所有路由器固件的合集：
> 链接:https://pan.baidu.com/s/1rUHbAQF6DPwHV10x3Ek4vg  密码:djxt

### 固件的实际使用体验：

padavan确实蛮好的，**默认是不支持小飞机的**，你需要用ssh连接上(账号是admin，密码是你的路由器管理员密码)，然后输入代码才可以开启，附上代码合集：
- 飞机：`nvram set google_fu_mode=0xDEADBEEF`
- KiwiVM:`nvram set ext_show_kiwivm_stat=1`
- KMS：`nvram set ext_show_lse=1`
- 闪讯：`nvram set ext_show_pppoesvr=1`
- 电信云加速：`nvram set ext_show_c189_speedup=1`
- Kcptun:`nvram set ext_show_kcptun=0`
- Shellinabox：`nvram set ext_show_siab=0`
- 保存：`nvram commit`

↑别忘记最后一条commit保存呀

然后发现它的小飞机体验贼差，基本用不了，上网搜了也有很多人有相同的问题。而且ipv6我配置好了似乎也有问题....

官改也挺好的，最新版有ipv6支持，然而我似乎还是用不了，小飞机体验完美。

其它我没试了....基本上折腾到这里就完了，总体感觉还是不错的，160的价格，有这么多功能，而且还是千兆的，血赚。
