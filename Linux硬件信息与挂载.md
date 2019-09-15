# Linux硬件信息与挂载
## 1. 硬件信息
### 1.1 CPU
```
cat /proc/cpuinfo
```
**查看CPU个数**  
```
cat /proc/cpuinfo | grep "physical id" | uniq | wc -l
```
**查看CPU核数**  
```
cat /proc/cpuinfo | grep "cpu cores" | uniq
```
**查看CPU型号**  
```
cat /proc/cpuinfo | grep 'model name' |uniq
```

### 1.2 内存
**内存总量**  
```
cat /proc/meminfo | grep MemTotal
```

### 1.3 磁盘
**磁盘分区大小/已用空间**  
```
df -h
```
**查看某目录大小**  
```
du -sh <目录名>
```
**查看当前目录下第一层文件和目录大小**  
```
du -h --max-depth=1
```
**查看当前目录以下文件和子目录大小**  
```
du -sh *
```
**查看磁盘类型**  
```
cat /etc/fstab
```

## 2. 挂载
### 2.1 NFS
**参考资料**
http://blog.huatai.me/2014/10/14/CentOS-7-NFS-Server-and-Client-Setup/
https://qizhanming.com/blog/2018/08/08/how-to-install-nfs-on-centos-7
**挂载NFS**
如果本地`/路径`目录还不存在，则`mkdir /路径`，然后执行：
```
mount -t nfs IP地址:/路径 /路径
```
**取消挂载NFS**
```
umount /路径
```
**查看挂载**
```
mount
df -Th
```
#### NFS常见问题：umount.nfs: XXX: device is busy
需要强制取消挂载，或杀进程
参考：https://blog.csdn.net/lanyang123456/article/details/55001333
```
umount -lf /路径
```



