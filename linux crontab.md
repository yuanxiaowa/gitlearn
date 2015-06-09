# Crontab 计划任务
- `crontab -l`: 检查Crontab工具是否安装
- `service crond status`: 检查crond服务是否启动
- `yum install vixie-cron`: 安装cron
- `yum install crontabs`: 安装crontab

## crontab 选项
- `-l`: 检查Crontab工具是否安装
`-e`: 编辑配置文件
`-u 用户`: 指定用户, 无则为当前用户

> `crontab 文件名`: 将文件的内容覆盖当前用户的配置文件

## crontab配置文件格式
* * * * *
1. 分钟(0-59)
2. 小时(0-23)
3. 日期(1-31)
4. 月份(1-12)
5. 星期(0-7)<0或者7表示周日>

> 当有多个值的时候使用,分隔

> 连续-

> `*/A` 表示每A分钟(小时等)执行一次命令, *为任意，也可以改为上面任何一种

> 日期和星期是或的关系

## 当前用户配置文件
`/var/spool/cron/root`
相当于 `crontab -l` 看到的

## 全局配置文件(系统级任务)
/etc/crontab
/etc/cron.d/sysstat

## 日志文件(执行记录)
/var/log/cron

## 发送给的用户配置文件
/var/spool/mail/

## 例子
- `*/1 * * * * date >> ~/log.txt`
- `0-58/2 * * * * echo "Event*****************"`
- `59 1 1-7 4 * test ``date +\%w`` -eq 0 && /root/a.sh`(%需要转义) 四月的第一个星期日早晨1时59分运行a.sh
- `0 */2 * * * 命令` 每两个小时执行一次
- `*/1 * * * * 命令;sleep 0.5s;命令` 使用shell让命令每半分钟执行一次