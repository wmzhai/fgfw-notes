# Ubuntu 16.04 shadowsocks+proxychains 安装及设置

## 1. 安装shadowsocks

```
apt install shadowsocks proxychains
```

## 2. 配置shadowsocks

新建文件

```
vi /etc/shadowsocks/shadowsocks.json
```

内容如下

```
{
  "server":"ip",
  "server_port":1984,
  "local_port":1080,
  "password":"密码",
  "timeout":600,
  "method":"aes-256-cfb"
}    
``` 

然后可以尝试启动

```
sslocal -c /etc/shadowsocks/shadowsocks.json &
```

## 3. 配置proxychains

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

## 4. shadowsocks设为systemd服务

### 4.1 新建配置

```
vi /lib/systemd/system/shadowsocks@.service
```

写入下面内容

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

### 4.2 启动服务

```
systemctl start shadowsocks@shadowsocks.service
systemctl start shadowsocks@cloudss.service
```

### 4.3 开机自启动

```
systemctl enable shadowsocks@shadowsocks.service
systemctl enable shadowsocks@cloudss.service
```

## 5. 使用方法

### 5.1 命名行前缀
用命令行启动软件，在前面加上proxychains，如：

```
proxychains firefox
```

### 5.2 新建bash

也可以通过输入
```
proxychains bash
```
建立一个新的shell，基于这个shell运行的所有命令都将使用代理。

