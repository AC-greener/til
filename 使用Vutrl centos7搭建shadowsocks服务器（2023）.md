### 使用ssh登录服务器

使用xshell或者mac的iterm连接vutrl的服务器，或者在Vultr的控制台直接登录服务器也行

### 安装shadowsocks

#### 1，安装wget

```shell
 yum install wget
```

#### 2，接着安装shadowsocks

```shell
wget --no-check-certificate -O shadowsocks.sh https://raw.githubusercontent.com/teddysun/shadowsocks_install/master/shadowsocks.sh

```

#### 3，获取 shadowsocks.sh 读取权限

```shell
chmod +x shadowsocks.sh

```

#### 4，设置你的 ss 密码和端口号

```shell
./shadowsocks.sh 2>&1 | tee shadowsocks.log
```

![image-20230306001553638](http://rqyeh2w9w.hn-bkt.clouddn.com/2023-03-06-120032.png)

#### 5，选择加密方式

![image-20230306001451144](http://rqyeh2w9w.hn-bkt.clouddn.com/2023-03-06-120036.png)

设置完密码和端口号之后，我们选择加密方式，这里选择 7 ，使用aes-256-cfb的加密模式：

#### 6，安装完成

等一会之后，就安装完成了，它会给你显示你需要连接 vpn 的信息：

![image-20230306001720630](https://p.ipic.vip/4m17z1.png)

可以看到需要连接 ss 的 ip地址，密码，端口，和加密方式。

最好吧这些信息保存一下

如果忘记可以在服务器上使用这个命令查看：

```shell
 cat  /etc/shadowsocks.json
```

![image-20230306195255023](https://p.ipic.vip/6sj7fl.png)

其中server_port就是你要使用端口号，password就是密码

### 防火墙处理

**注意**：下面每个步骤的端口号需要换成第4步里面设置的端口号

1，检查你设置的端口是否被防火墙拦截，这里的8388需要替换成第4步里面设置的端口号

```shell
firewall-cmd --list-ports | grep 8388
```

该命令将列出所有允许通过防火墙的端口，并搜索端口8388。如果该端口被允许，它将被显示在输出中。

如果输出是空的，说明8388端口不允许通过防火墙。

![image-20230306193545703](https://p.ipic.vip/7v8jwo.png)

2，允许访问8388端口

```shell
firewall-cmd --zone=public --add-port=8388/tcp --permanent
```

这条命令将把8388端口添加到公共区域，然后需要重新加载防火墙配置

```shell
firewall-cmd --reload
```

看到success就表示成功了



![image-20230306194046498](https://p.ipic.vip/aq5ekb.png)

### 使用shadowsocks

安装地址

windows：https://github.com/shadowsocks/shadowsocks-windows/releases

<img src="https://p.ipic.vip/leld9r.png" alt="image-20230306194813973" style="zoom: 33%;" />

macOS：https://github.com/shadowsocks/shadowsocks-iOS/releases

<img src="https://p.ipic.vip/yttc26.png" alt="image-20230306194858314" style="zoom:33%;" />

其他操作系统可以在这里查找：https://github.com/shadowsocks

安装完成之后打开shadowsocks设置服务器，填入第6步的信息

<img src="https://p.ipic.vip/0pppo3.png" alt="image-20230306195459565" style="zoom:50%;" />

选择刚才的服务器就可以使用了

![image-20230306200024581](https://p.ipic.vip/arobjw.png)