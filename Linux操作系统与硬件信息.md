# Linux操作系统与硬件信息
## 1. 操作系统
**操作系统版本**  
```
cat /proc/version
cat /etc/redhat-release
cat /etc/*elease
```
**操作系统内核信息**  
```
uname -a
```

## 2. 硬件信息
### 2.1 CPU
```
cat /proc/cpuinfo
```
**查看CPU个数**  
```
cat /proc/cpuinfo | grep "physical id" | uniq | wc -l
```
**查看CPU核数**  
```
cat /proc/cpuinfo | grep "cpu cores" | uniq
```
**查看CPU型号**  
```
cat /proc/cpuinfo | grep 'model name' |uniq
```

### 2.2 内存
**内存总量**  
```
cat /proc/meminfo | grep MemTotal
```

### 2.3 磁盘
**磁盘分区大小/已用空间**  
```
df -h
```
**查看某目录大小**  
```
du -sh <目录名>
```
**查看当前目录下第一层文件和目录大小**  
```
du -h --max-depth=1
```
**查看当前目录以下文件和子目录大小**  
```
du -sh *
```
**查看磁盘类型**  
```
cat /etc/fstab
```