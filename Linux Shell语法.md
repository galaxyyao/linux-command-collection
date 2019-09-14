# Linux Shell
## 1. 基本语法与数据结构
### 1.1 if...else
```shell
if [ command ]; then
    # do something
fi
```
**if中两个条件**  
```shell
if [ "$#" -eq 0 ] || [ "$#" -gt 1 ] ; then
    echo "hello"
fi
```

**多条件分支**  
```shell
if ... elif ... else ... fi
```

### 1.2 for循环
```
array=("A" "B" "ElementC" "ElementE")
for element in "${array[@]}"
do
    echo "$element"
done
```

### 1.3 数组
**数组元素个数**  
```
${#array[@]}
```

### 1.4 比较
**整数比较**  
- -eq 等于,如:if [ "$a" -eq "$b" ]
- -ne 不等于,如:if [ "$a" -ne "$b" ]
- -gt 大于,如:if [ "$a" -gt "$b" ]
- -ge 大于等于,如:if [ "$a" -ge "$b" ]
- -lt 小于,如:if [ "$a" -lt "$b" ]
- -le 小于等于,如:if [ "$a" -le "$b" ]
- `<`   小于(需要双括号),如:(("$a" < "$b"))
- `<=`  小于等于(需要双括号),如:(("$a" <= "$b"))
- `>`   大于(需要双括号),如:(("$a" > "$b"))
- `>=`  大于等于(需要双括号),如:(("$a" >= "$b"))

**字符串比较**  
- var1 = var2     字符串相等
- var1 != var2    字符串不等
- var1 < var2     小于
- var1 > var2     大于
- -n var1             长度大于0
- -z var1             长度等于0

### 1.5 运算
**整数加法**  
```
a=$(( $b+$c ))
```

## 2. 常用脚本
**检测Java是否有安装**  
https://stackoverflow.com/questions/7334754/correct-way-to-check-java-version-from-bash-script  

**检查包是否有安装**  
```shell
if rpm -qa | grep -q 包名 ; then
  do something
fi
```

**判断文件是否存在**  
```shell
if [ ! -f /tmp/foo.txt ]; then
    echo "File not found!"
fi
```

**判断目录是否存在**  
```shell
if [ ! -d /tmp/dir ]; then
    echo "Directory not found!"
fi
```

**判断变量是否存在**  
```shell
if [ -n "$var" ]; then
  echo "You don't supply the variable!"
fi
```

**文件MD5**  
```shell
md5=`md5sum package-lock.json | awk '{ print $1 }'`
```

**命令输出到变量**  
```shell
v=`java -version`
echo $v
```

**字符串替换**  
```
${var//要替换的字符串/替换目标字符串}
```

**写文本到文件**  
```shell
cat > FILE.txt <<EOF
info code info
info code info
info code info
EOF
```

**当前时间**  
```shell
now=$(date +%T)
```

## 3. Expect自动化交互
参考资料：  
https://www.jianshu.com/p/70556b1ce932  
https://www.cnblogs.com/lzrabbit/p/4298794.html  
- send:向进程发送字符串，用于模拟用户的输入。注意一定要加\r回车
- expect:从进程接收字符串
- spawn:启动进程(由spawn启动的进程的输出可以被expect所捕获)
- interact:用户交互

**范例1：远程登录输入密码并创建目录**  
```shell
#!/usr/bin/expect
set timeout -1
spawn ssh root@10.16.34.125
expect {
    "password" {send "axxt600816\r";}
    "yes/no" {send "yes\r";exp_continue}
}
expect "root" {send "mkdir testExpect\r"}
send "exit\r"
expect eof
exit
```
