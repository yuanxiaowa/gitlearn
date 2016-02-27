# iptables

## 配置文件
/etc/sysconfig/iptables

## iptables中的4表5链
- 四张表：filter表、nat表、mangle表、raw表
- 五条链： INPUT OUTPUT FORWARD PREROUTING POSTROUTING

## 查看规则列表
- `iptables -L` 如果能识别端口应用程序，则显示应用程序名称
- `iptables -nL` 显示端口
- `iptables -F` 清理规则

## 查看本机端口使用情况
`netstat -luntp`

## 查看路由列表
`netstat -rn`

## 将内存中的iptables保存到配置文件中
`/etc/init.d/iptables save` 或 `service iptables save`

## 将服务设置成开机启动
`chkconfig iptables on`

## 查看服务列表情况
`chkconfig --list`

## 开机自动运行的脚本文件
/etc/rc.local

## 端口扫描工具
nmap
> `nmap -sS -p 1-65535 10.0.0.112` 扫描 1-65535 端口

## vsftpd（ftp服务器，若没有则需要安装）
/etc/vsftpd/vsftpd.conf
- `port_enable=yes`
- `connect_from_port_20=YES`
- `pasv_min_port=5000` 被动模式传输端口范围
- `pasv_max_port=6000` 被动模式传输端口范围

## ftp匿名用户
anonymous

## 内核
`modprobe 模块名`
- `modprobe nf_conntrack_ftp`: 加载ftp的连接追踪模块，将ftp连接改为被动模式
- `modprobe -l`: 查看内核加载了的模块

## 开机自动加载模块的配置文件
`/etc/sysconfig/iptables-config` 将其中的 `IPTABLES_MODULES=""` 里面指定要加载的模块

## 内核参数文件
`/etc/sysctl.confg`
- 允许数据包的转发需要将 `net.ipv4.ip_forward = 1` 0表示不转发
- `sysctl -p`: 应用规则
- `sysctl -a`: 查看规则列表
- `sysctl -w net.ipv4.icmp_echo_ignore_all=1`: 忽略icmp请求，设置之后就不能ping通
## 定义转发路由
/etc/sysconfig/network
> 修改 GATEWAY 为要转发的IP

或者 `rout add 0.0.0.0 gw 10.0.0.112`


## connlimit 模块
用于限制每一个客户端IP的并发连接数

## limit 模块
限速，控制流量
- `--limit-burst` 默认为5

## 例子
- 开放端口(范围表示使用:分隔，多个使用,分隔)： `-I INPUT -p tcp --dport 80 -j ACCEPT`
- 本机可以访问本机(本地回环)： `-I INPUT -i lo -j ACCEPT`
- 让本机可以访问外部请求： `-I INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT`
- 只允许某个IP访问某个端口： `-I INPUT -p tcp -s 10.0.0.112 --dport 800 -j ACCEPT`
- 允许某个网段访问: `-A INPUT -s 10.0.155.0/24 -j ACCEPT`
- 允许不同网段间的数据包转发到某台机器: `iptables -t nat -A POSTROUTING -s 10.0.177.0/24 -j SNAT --to 10.0.0.112`
- 将发往10.0.0.112:80端口(本机)的请求转发到10.0.0.1.112:80: `iptables -t nat -A PREROUTING -d 10.0.0.112 -p tcp --dport 80 -j DNAT --to 10.0.0.1.112:80`
- 加载connlimit模块，限制连接数: `iptables -I INPUT -p TCP --syn --dport 80 -m connlimit --connlimit-above 10 -j REJECT`
-	加载limit模块，限速，控制流量
	- 每小时允许3次连接: `iptables -A INPUT -m limit --limit 3/hour`
	-	超出10次后，每分钟只允许1次连接
		- `iptables -A INPUT -p icmp -m limit --limit 1/m --limit-burst 10 -j ACCEPT`
		- `iptables -A INPUT -p icmp -j DROP`