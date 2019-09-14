# Linux文件操作
# 1. 文件内容查看
## 1.1 cat
```
cat 文件名
```
**cat查看（分页显示结果）**  
```
cat 文件名 | less
```
（中止按q）  

**cat其他用法**  
文件创建
```shell
cat > test.txt
```

## 1.2 head & tail
**从头开始看n行**  
```shell
head -n 文件名
```
**从尾开始看n行**  
```
tail -n 文件名
```

# 2. 文件内容检索
**在文件里搜索的字符串**  
```shell
grep "word" filename
```
**显示关键字下的N行（范例为10行）**  
```shell
grep -A 10 "word" crm-backend.log
```
**显示关键字上下的N行（范例为5行）**  
```shell
grep -C 10 "word" crm-backend.log
```

**在目录里搜索包含关键字的文件**  
```shell
grep -rl "word" /path
```
- -r表示--recursive
- -l表示--files-with-matches

# 3. 流操作sed
sed一般使用单引号，sed引用shell变量时使用双引号即可，因为双引号是弱转义，不会去除$的变量表示功能，而单引号为强转义，会把$作为一般符号表示，所以不会表示为变量。  
**全局文本内容替换**  
```shell
sed -i "s/被替换字符串/替换目标字符串/g" 文件名
```
**移除开头第N行**  
```shell
sed -i -e '1,1 d' 文件名
```
**开头添加行**  
```shell
sed -i '1s/^/列1,列2\n/' 文件名
```
**移除BOM头**  
```shell
sed -i '1s/^\xEF\xBB\xBF//' 文件名
```
**添加BOM头**  
```shell
sed -i '1s/^/\xEF\xBB\xBF/' 文件名
```

**参考资料**  
https://coolshell.cn/articles/9104.html  

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