# Linux时间与计划任务
## 1. 时间
**当前时间**  
```
date
```
**查看硬件时间**  
```
hwclock --show
```
**获得命令执行时间**  
```
time 命令
```
例如：`time date`

**手动修改时间**  
```
date -s 13:00:00
```
**手动修改日期**  
```
date +%Y%m%d -s "20170101"
```

## 2. NTP始终同步
**ntp同步时间**  
```
ntpdate cn.pool.ntp.org
```

### 2.1 搭建局域网时间同步服务器+定时同步
**服务器端**  
```
yum -y install ntp
vi /etc/ntp.conf
```
添加内容：  
```
server ntp1.aliyun.com
server time.nist.gov
```
启动ntpd服务：  
```
systemctl start ntpd
systemctl enable ntpd
ntpq -p
firewall-cmd --zone=public --add-port=123/udp --permanent
firewall-cmd --reload
```

**客户端**  
手动同步：  
```
ntpdate -d 服务器端ip
```

自动同步：
```
crontab -e
```
添加内容
```
0 4 * * * ntpdate 服务器端ip >/dev/null 2>&1
```

## 3. 时区
**参考资料**  
http://www.cnblogs.com/kerrycode/p/4217995.html  

**查看时区**  
```
date -R
```
**改当前用户时区**  
```
export TZ='Asia/Shanghai'
source ~/.bashrc
```
**改系统时区**  
```
timedatectl set-timezone 'Asia/Shanghai'
timedatectl # 确认
```

## 4. 计划任务
**查看系统计划任务**  
```
vi /etc/crontab
```
或
```
vi /etc/anacrontab
```
**按频率分类**
```
/etc/cron.hourly
/etc/cron.daily
/etc/cron.weekly
/etc/cron.monthly
```

注意：  
`/etc/crontab` ≠ `crontab -e`

### 4.1 crontab排查
crontab中经常配置运行脚本输出为：`>/dev/null 2>&1`，来避免crontab运行中有内容输出  
如果要排查，可以改为`&>/var/log/xxx.log`  
注意crontab的PATH重新定义过了，可以通过`vi /etc/crontab`查看

**查看crontab错误日志**  
```
tail -100f /var/log/cron
```
**查看crontab日志**  
```
tail -100f /var/spool/mail/root
```

**配置crontab**  
```
crontab -e
```

### 4.2 crontab格式
```
24 09 * * * /usr/local/scrapy/amac_spider/amac_spider.sh
```
格式为：minute hour day-of-month month day-of-week command
```
# Example of job definition:
# .---------------- minute (0 - 59)
# |  .------------- hour (0 - 23)
# |  |  .---------- day of month (1 - 31)
# |  |  |  .------- month (1 - 12) OR jan,feb,mar,apr ...
# |  |  |  |  .---- day of week (0 - 6) (Sunday=0 or 7) OR sun,mon,tue,wed,thu,fri,sat
# |  |  |  |  |
# *  *  *  *  * user-name  command to be executed
```
