# Linux网络管理

## IP地址分类
网络类别 | 最大网络数 | IP地址范围 | 最大主机数 | 私有IP地址范围
-------- | ---------- | ---------- | ---------- | --------------
A | 126(2^7-2) | 1.0.0.0-126.255.255.255 | 2^24-2 | 10.0.0.0-10.255.255.255
B | 16384(2^14) | 128.0.0.0-191.255.255.255 | 2^16-2 | 172.16.0.0-172.31.255.255
C | 2097152(2^21) | 192.0.0.0-223.255.255.255 | 2^8-2 | 192.168.0.0-192.168.255.255

## 子网掩码
* A类为255.0.0.0
* B类为255.255.0.0
* C类为255.255.255.0

> A类可以是B类或C类的子网掩码
>
> B类可以分配C类的子网掩码

## 端口作用
常用端口
* FTP(文件传输协议): 20 21
* SSH(安全shell协议): 22
* telnet(远程登录协议): 23
* DNS(域名系统): 53
* http(超文本传输协议): 80
* SMTP(简单邮件传输协议): 25
* POP3(邮局协议3代): 110

### `netstat 选项`
查看网络状态
* `-t`: 列出TCP协议端口
* `-u`: 列出UDP协议端口
* `-l`: 仅列出在监听状态网络服务
* `-a`: 查看所有连接和监听端口
* `-n`: 显示IP地址和端口号，而不显示域名和服务名
* `-r`: 列出路由列表，功能和route命令一致

> ESTABLISHED 代表连接已经存在，通常为远程连接强制关闭时候出现

### `route 选项`
可以看到网关
* `-n`: 显示IP地址和端口号，而不显示域名和服务名

>
> * `route add default gw 192.168.1.1` 添加默认网关
> * `route del default gw 192.168.1.1` 删除网关

>> 不能乱设网关，外网的网卡才能设网关

### `nslookup 主机名或IP`
进行域名与IP地址解析
> 直接回车，输入对应的名字可以查看其详情 `server`: 查看本机DNS服务器

### `ping 主机名或IP`
探测指定IP或域名的网络状况
* `-c 次数`: 指定ping包的次数

### `telnet [域名或IP] [端口]`
远程管理与端口探测命令(windows和linux中默认没有开启)

### `traceroute [选项] IP或域名`
路由跟踪命令
* `-n` 使用IP, 不适用域名, 速度更快

### `wget [url]`
下载命令

### `tcpdump -i eth0 -nnX port 21`
选项
* `-i` 指定网卡接口
* `-nn` 数据包中的域名与服务转为IP和端口
* `-X` 以十六进制和ASCII码显示数据包
* `port` 指定监听端口

> telnet和ftp都是明文访问协议

## 网关
> 交换机(网内进行数据交换) 路由器(网间)

> 内网通信可以不配网关和dns

## IP地址配置
* ifconfig命令临时配置IP地址
	
	ifconfig eth0 192.168.0.200 netmask 255.255.255.0

* setup工具永久配置IP地址(redhat系列专有命令)
*	修改网络配置文件 `/etc/sysconfig/network-scripts/ifcfg-eth0`
	* `DEVICE=eth0` 网卡设备名
	* `BOOTBROTO=none` 是否自动获取IP(none, static, dhcp)
	* `HWADDR=00:0c:29:17:c4:09` MAC地址
	* `NM_CONTROLLED=yes` 是否可以由Network Manager图形管理工具托管
	* `ONBOOT=yes` 是否随网络服务启动,eth0生效
		
		> centos 6以上需要手动改为yes
	
	* `TYPE=Ethernet` 类型为以太网
	* `UUID=` 唯一识别码
		
		> 两台计算机相同的话都不能上网，复制时候需要注意
		> 
		> 解决方法
		>
		> 1. 删除MAC地址行
		> 2. 删除网卡网卡和MAC地址绑定文件 `rm -rf /etc/udev/rules.d/70-persistent-net.rules`
		> 3. 重新启动系统
		
	* `IPADDR=192.168.0.252` IP地址
	* `NETMASK=255.255.255.0` 子网掩码
	* `GATEWAY=192.168.0.1` 网关
	* `DNS1=202.106.0.20` DNS
	* `IPV6INIT=no` IPv6没有启用
	* `USERCTL=no` 不允许非root用户控制此网卡
	
	> 重启 service network restart
	
* 图形界面配置IP地址

## 主机名文件
`/etc/sysconfig/network`
* `NETWORKING=yes` 网络是否工作
* `HOSTNAME=localhost.localdomain` 主机名

> 查看与临时设置主机名命令 `hostname [主机名]`

## DNS配置文件
`/etc/resolv.conf`

* `nameserver 202.106.0.20`
	
	> 若有多个则使用空格分隔 或 换行重写
	
* `search localhost` 域名补全时候用，若域名不全，则在后面自动补.localhost

## 虚拟机网络配置
* 桥接: 需要占用真实网段中的一个ip
* NAT: 使用8网卡进行通信, 只能与真实机进行通信, 能访问公网, 但不能与内网中的其他计算机通信
* Host only: 使用1网卡进行通信, 只能与真实机进行通信, 不能与内网中的其他计算机和公网进行通信

## 网络命令
* `ifconfig`
* `ifdown 网卡设备名` 和 `ifup 网卡设备名` 禁用和启用网卡

## 远程登录工具
*	ssh
	* `ssh 用户名@ip` 远程管理指定Linux服务器
	* `~/.ssh/kown_hosts` 公钥保存位置
*	scp
	* `scp [-r] 用户名@ip:文件路径 本地路径`: 下载文件
	* `scp [-r] 本地路径 用户名@ip:文件路径`: 上传文件
	
*	工具
	* SecureCRT(远程管理工具)
	* Xshell(远程管理工具)
	* WinSCP(windows和Linux文件传输工具)