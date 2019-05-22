### 前言
git clone太慢，一般速度只有20k/s，一般的小项目还凑合，大一点的就跪了。。考虑用git自带的代理配合自己的SS进行代理加速

### 解决方法
```
git config --global http.proxy socks5://127.0.0.1:1086
git config --global https.proxy socks5://127.0.0.1:1086
```
需要注意的是代理的端口，根据自己的ss进行修改即可。这两句下去应该就很快了
