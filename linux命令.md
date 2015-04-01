#linux常用命令


## 命令基本格式以及文件处理命令
### cd
* cd ~
* cd：进入家目录
* cd -：进入上次目录
* cd ..：进入上级目录
* cd .：进入当前目录

### ls [选项] [文件或目录]
- -a： 显示所有文件，包括隐藏文件 
- -l：显示详细信息
- -d：查看目录属性
- -h：人性化显示文件大小
- -i：显示inode

### ll
ls -l（这条命令的别名）

### pwd
查询当前目录位置

### touch 文件名
新建一个空文件

### mkdir -p [目录名]
- -p：递归创建
	
### rmdir [目录名]
只能删除空白目录

### rm -rf [文件名]
* -r：删除目录
* -f：强制删除
	
### cp [原文件或目录] [目标目录]
- -r：复制目录
- -p：连带文件属性复制
- -d：若源文件是链接文件，则复制链接属性
- -a：相当于\-pdr

### mv [原文件或目录] [目标目录] 
### ln [原文件] [目标文件]
生成链接文件
- \-s：创建软链接（创建软链接的时候，如果不在同一目录，需要使用绝对路径）


	
## 压缩与解压缩
* \.zip

	### zip 压缩文件名 源文件
		压缩文件
	### zip -r 压缩文件名 源目录
		压缩目录
	### unzip 压缩文件
		解压缩.zip文件
* .gz
	### gzip 源文件
		压缩为.gz格式的压缩文件，源文件会消失
	### gzip -c 文件 > 压缩文件
		压缩为.gz格式，源文件保留
	### gzip -r 目录
		压缩目录下所有的子文件，但是不能压缩目录
	### gzip -d 压缩文件
		解压缩文件
	### gunzip 压缩文件
		解压缩文件
* .bz2，不能压缩目录
	### bzip2 源文件
		压缩为.bz2格式，不保留源文件（加-k保留源文件）
	### bzip2 -d 压缩文件
		解压缩（加-k保留源文件）
	### bunzip2 压缩文件
		解压缩（加-k保留源文件）
* 打包文件
	### tar -cvf 打包文件名 源文件
		打包 -c：打包 -v：显示过程 -f：指定到爆后的文件名
	### tar -xvf 打包文件名
		解打包 -x：解打
	### tar -tvf 打包文件名
		查看压缩包里面的文件
	### tar -zcvf 压缩包名.tar.gz 源文件..
		打包压缩
	### tar -zxvf 压缩包名.tar.gz （-C 解压到的文件目录）
		解压缩解打
	### tar -jcvf 压缩包名.tar.bz2 源文件..
		打包压缩
	### tar -jxvf 压缩包名.tar.bz2 （-C 解压到的文件目录）
		解压缩解打
		

## 文件搜索命令


## 关机和重启命令
	### shutdown [选项] 时间
		-c：取消前一个关机命令 -h：关机 -r：重启
		* shutdown -c
		* shutdown -h 00:00
		* shutdown -h now
	### halt，poweroff，init 0
		关机（不安全）
	### init 6
		重启
	### runlevel
		查询系统的运行级别
	### /etc/inittab
		系统运行级别配置文件
	### logout
		退出

## 网络命令
	### /etc/sysconfig/network-scripts/ifcfg-eth0
		网络地址配置文件
	### /etc/sysconfig/iptables
		防火墙配置文件（可以指定开放哪些端口）
	### service iptables restart/start/stop
		重启/开启/停止防火墙服务
	### chkconfig iptables on/off
		防火墙开放/关闭（重启后生效）