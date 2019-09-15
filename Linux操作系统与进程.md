# Linux操作系统与进程
## 1. 进程
**查看进程间父子关系**  
```
ps auxf
```

**树状图的方式展现进程之间的派生关系**  
```
pstree -a
```

**查看进程开始和持续时间**  
```
ps -p <PID> -o lstart,etime
```

**杀进程**  
```
kill -9 -f 进程id
```
也可以用pkill

**杀掉运行时间超过5分钟的进程**  
```shell
kill -9 $(ps -eo comm,pid,etimes | awk '/^procname/ {if ($3 > 300) { print $2}}')
```

**根据参数杀掉时间超过1小时的进程**  
https://www.unix.com/shell-programming-and-scripting/240527-kill-long-running-script-if-crosses-threshold-time.html  

## 2. 服务器操作
立即关机
```
shutdown -h now
```
重启
```
shutdown -Fr now
```
或
```
init 6
```
或在系统出问题情况下或强制重启下用reboot  
```
systemctl reboot -i
```

**开机启动**  
```
vi /etc/rc.local
```
该脚本是在系统初始化级别脚本运行之后再执行的，因此可以安全地在里面添加你想在系统启动之后执行的脚本。  

## 3. 环境变量
**打印环境变量**  
```
printenv
```
或
```
env
```

**打印某个环境变量**  
```
echo $PATH
```

**修改当前会话的环境变量**  
范例：修改PATH
```
export PATH=$PATH:/opt/scripts
```

**修改当前用户的环境变量**  
```
vi ~/.bashrc
PATH={$PATH}:/opt/scripts
```

**修改所有用户的环境变量**  
```
修改~/.bash_profile
```

**添加系统环境变量**  
```
vi /etc/environment
```
添加内容：`KEY=VALUE`，然后执行：  
```
source /etc/environment
```

## 4. 操作系统信息
**操作系统版本**  
```
cat /proc/version
cat /etc/redhat-release
cat /etc/*elease
```
**操作系统内核信息**  
```
uname -a
```

## 5. systemd
https://www.ibm.com/developerworks/cn/linux/1407_liuming_init3/index.html  

## 6. 系统文件句柄限制
**取消文件句柄限制**  
ulimit -a 

