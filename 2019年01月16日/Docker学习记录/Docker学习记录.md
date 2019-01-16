> docker run IMAGE [command][ARG...]
一次运行只运行一次服务的容器
	举例：docker run ubuntu echo 'Hello world'
	

> docker run -i -t IMAGE /bin/bash
-i == interactive
-t == tty
提供一个交互式的容器，使用exit退出
> docker run -i -t ubuntu /bin/bash
	

> docker ps [-a][-l]
	docker ps 
	查看正在运行的容器
	docker ps -a
	查看所有的容器
	docker ps -l
	列出最新创建的容器

> docker inspect [containerid][name]
	返回json格式的数据
	举例：docker inspect b1f7430eb2ca
	举例：docker inspect mycontainer

> docker run --name=自定义名字 IMAGE [command][ARG...]
创建一个自定义名字的容器
	举例：docker run --name=mycontainer ubuntu echo 'hello world'

> docker start [-i] 容器名
	重新启动容器，i选项是交互式

> docker rm 容器名
	只能删除已经停止的容器

> docker run -i -t IMAGE /bin/bash  ctrl+p ctrl+q 
	运行守护式容器，一直在后台运行

> docker attach 容器名
	再次进入后台运行的容器

> docker run -d IMAGE [command][ARG...]
	-d == 启动容器时用后台运行，命令结束后容器依然会停止

> docker logs [-f][-t][--tail] 容器名
	-f == follows 一直更新
	-t == timestamps 是否显示时间戳
	--tail 从尾部开始几个，0=最新

> docker top 容器名
	查看运行中容器的进程

> docker exec [-d][-i][-t] 容器名 IMAGE [command][ARG...]
	在容器中启动新的进程

> docker stop 容器名
	停止容器

> docker kill 容器名
	马上停止，不管你在做什么

man docker-run
man docker-logs
man docker-top
等等

###在docker中部署静态网站

1. 设置容器的端口映射
> docker run [-P][-p]
	-P == publish all 所有的端口暴露
	-p == 指定端口
	docker run -P -i -t ubuntu /bin/bash
	docker run -p 80 -i -t ubuntu /bin/bash 宿主机的端口是随机映射的
	docker run -p 8080:80 -i -t ubuntu /bin/bash 同时指定宿主机的和容器的端口
	docker run -p ip:80 -i -t ubuntu /bin/bash 指定ip
	docker run -p ip:8080:80 -i -t ubuntu /bin/bash 

2. 部署Nginx
1.创建映射80端口
2.安装Nginx
3.安装vim
4.创建静态页面
5.修改Nginx
6.运行Nginx
7.访问你的静态网页

> docker run -p 80 --name web -i -t ubuntu /bin/bash
  主机映射到容器80端口

>apt-get update
apt-get install nginx
apt-get install vim

>cd /var/www/html
随便创建一个静态页面

> whereis nginx  然后查看配置文件，我是不用改的
 nginx 运行nginx服务
 ps -ef 查看进程

记得用p+q退出

> docker port 容器名 查看docker映射的情况

> curl http://127.0.0.1:32768  这个端口使用映射查到的

> docker inspect web |grep IPAddress 可以用这个ip地址显示 但是我失败了
容器重新启动后Nginx就自动关闭了
docker exec web nginx 重启Nginx服务，但是容器的映射关系会发生变化






















