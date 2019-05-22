基本上我的nmap扫描就是为了能够探测某台主机开放的端口和某个端口是否开放，做个记录

`nmap -Pn www.baidu.com`   扫描百度开放的端口
`sudo nmap -sV -O -F www.baidu.com`   快速扫描主机服务端口版本及系统类型
`nmap -Pn www.baidu.com -p 80` 查询某台主机的某个**TCP**端口是否开放
`sudo nmap -sU www.baidu.com -p 80` 查询某台主机的某个**UDP**端口是否开放
