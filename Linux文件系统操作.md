# Linux文件系统操作
## 1. 目录查询
### 1.1 目录查询
**目录查询**  
```
ls
```
或  
```
ll
```
**按时间顺序查询**  
```
ls -lt
```
**按时间倒序查询**  
```
ls -lrt 
```

### 1.2 进入目录
```
cd 目录
```
**进入上一层目录**  
```
cd ../
```

### 1.3 查看目录属性
**查看当前目录大小**  
```
du -h --max-depth=1
```
**查看某个目录大小**  
```
du -hs 目录路径
```
**将目录下文件按大小（M）排序**  
```
du -sh ./* | sort -rn
```
**统计某文件夹下文件的个数**  
```
ls -l |grep "^-"|wc -l
```
**统计某文件夹下目录的个数**  
```
ls -l |grep "^ｄ"|wc -l
```

## 2. 文件查询
### 2.1 搜索
**按关键字搜索文件名**  
find . -name "*关键字*"

### 2.2 查询文件散列值
```
md5sum 文件名
sha1sum 文件名
```

## 3. 文件与目录操作
### 3.1 删除文件/目录
```
rm -rf 文件名
```
**除了某个文件都移除**  
```
rm -rf !(test.sh)
```
移除最近修改时间超过N天的文件
find /path/to/files* -mtime +5 -exec rm -rf {} \;

删除N天前的日志文件
find ./my_dir -mtime +10 -type f -delete

删除N天前的目录
find /mydir/  -maxdepth 1 -type d  -name "2018*" -exec rm -rf {} \;

删除swap文件
find . -type f -name "*.sw[klmnop]" -delete

复制
复制文件夹
cp -r /home/hope/files/* /home/hope/backup
复制并不弹出提示
yes | cp -r /home/hope/files/* /home/hope/backup
创建文件夹
mkdir
创建多层目录
mkdir -p /dir1/dir2/dir3

软链接
添加软链接
ln -s source_path target_path
加入PATH启动
ln -s <二进制文件路径> /usr/local/sbin
删除软链接
rm ln_path
（不能加/）

NFS
资料
http://blog.huatai.me/2014/10/14/CentOS-7-NFS-Server-and-Client-Setup/
https://qizhanming.com/blog/2018/08/08/how-to-install-nfs-on-centos-7

挂载NFS
如果本地“/路径”目录还不存在，则mkdir /路径
mount -t nfs IP地址:/路径 /路径
取消挂载NFS
umount /路径
查看挂载
mount
df -Th

常见问题
umount.nfs: XXX: device is busy
需要强制取消挂载，或杀进程
https://blog.csdn.net/lanyang123456/article/details/55001333
umount -lf /路径

文件名加.d
文件名加.d一般表示该目录包含一系列配置文件。例如/etc/nginx/conf.d，/etc/conf.d