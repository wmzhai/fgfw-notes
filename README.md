# Ubuntu 16.04 shadowsocks+proxychains 安装及设置

http://blog.csdn.net/szsteel1/article/details/54773544

## 安装shadowsocks

```
# apt install shadowsocks proxychains
```

## 配置shadowsocks

新建文件/etc/shadowsocks/shadowsocks.json，内容如下：

```
{
  "server":"服务器名",
  "server_port":服务器端口号,
  "local_port":1080,
  "password":"密码",
  "timeout":600,
  "method":"加密方式"
}    
``` 

## 启动shadowsocks

```
# sslocal -c /etc/shadowsocks/shadowsocks.json &
```

## 配置proxychains

编辑/etc/proxychains.conf，最下面有一行
```
socks4 127.0.0.1 9050
```
把这一行注释掉，添加一行
```
socks5 127.0.0.1 1080
```
然后测试
```
proxychains curl www.google.com
```




## shadowsocks设置为开机自动加载的systemd服务

### 新建服务文件

在/lib/systemd/system目录下新建一个文件shadowsocks@.service

```
[Unit]
Description=Shadowsocks Client Service
After=network.target

[Service]
Type=simple
User=nobody
ExecStart=/usr/bin/sslocal -c /etc/shadowsocks/shadowsocks.json

[Install]
WantedBy=multi-user.target
```

### 启动shadowsocks服务

```
# systemctl start shadowsocks@shadowsocks.service
# systemctl start shadowsocks@cloudss.service
```

### 设置开机自启动

```
# systemctl enable shadowsocks@shadowsocks.service
# systemctl enable shadowsocks@cloudss.service
```

## 使用方法

### 命名行前缀
用命令行启动软件，在前面加上proxychains，如：

```
proxychains firefox
```

### 新bash

也可以通过输入
```
proxychains bash
```
建立一个新的shell，基于这个shell运行的所有命令都将使用代理。

