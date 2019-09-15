# Linux文件与目录操作
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

### 1.3 创建目录
```
mkdir
```
创建多层目录  
```
mkdir -p /dir1/dir2/dir3
```

### 1.4 查看目录属性
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
```
find . -name "*关键字*"
```

### 2.2 查询文件散列值
```
md5sum 文件名
sha1sum 文件名
```

## 3. 文件与目录操作
### 3.1 删除
```
rm -rf 文件名
```
**除了某个文件都移除**  
```
rm -rf !(test.sh)
```
**移除最近修改时间超过N天的文件**  
```shell
find /path/to/files* -mtime +5 -exec rm -rf {} \;
```

**删除N天前的以2018开头的目录**  
```shell
find /mydir/  -maxdepth 1 -type d  -name "2018*" -exec rm -rf {} \;
```

**删除swap文件**  
```shell
find . -type f -name "*.sw[klmnop]" -delete
```

### 3.2 复制
**复制目录**  
```shell
cp -r /home/hope/files/* /home/hope/backup
```

# 4. 压缩/解压缩
## 4.1 zip
**将目录压缩为zip**  
```shell
zip -r myfile.zip mydir
```
**解压缩zip**  
```
unzip 文件名.zip
```

## 4.2 tar
**解压缩tar**  
```
tar -xvf 文件名.tar
```

**解压缩tar.gz**  
```
tar zxvf 文件名.tar.gz
```
**压缩成tar.gz**  
```
tar -zcvf 文件名.tar.gz 目录
```

**解压缩tar.xz**
```
tar xf 文件名.tar.xz
```

## 4.3 gzip
**压缩并删除原始文件**  
```
gzip 文件名
```
**压缩并保留原始文件**  
```
gzip -c 文件名
```

**gzip解压缩并删除原始文件**
```
gunzip 文件名
```
**gzip解压缩并保留原始文件**
```
gunzip -c 文件名
```

## 5. 软链接
**添加软链接**  
```
ln -s source_path target_path
```
可以通过将可执行程序在/usr/bin下建立软链接，就可以在任何路径下执行了  

**删除软链接**  
```
rm ln_path
```
注意：不能加`/`  