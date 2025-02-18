# Linux终端命令
# 1. 打印
**字符串打印到屏幕**
```
echo（-e为激活转义字符）
```
**echo带颜色输出**  
参考资料：http://blog.51cto.com/1inux/1634799  

**红色字体**  
```
echo -e "I \033[31mLOVE\033[0m CHINA"
```
**红色底色**  
```
echo -e "I \033[41mLOVE\033[0m CHINA"
```

**终端支持256色**  
参考资料：http://www.cnblogs.com/yangyangup/p/5146861.html  
```
tput colors
```
如果不支持xterm-256color，则用`yum install ncurses`，然后`export TERM='xterm-256color'`

# 2. 输出
- 1 表示stdout标准输出，系统默认值是1，所以`>/dev/null`等同于`1>/dev/null`
- 2 表示stderr标准错误

`1>/dev/null` 表示标准输出重定向到空设备文件，也就是不输出任何信息到终端  
`2>&1` 表示标准错误输出重定向等同于标准输出  
`>/dev/null 2>&1` 因为之前标准输出已经重定向到了空设备文件，所以标准错误输出也重定向到空设备文件  

# 3. 显示命令属性
**type**  
- -t：输出“file”、“alias”或者“builtin”，分别表示给定的指令为“外部指令”、“命令别名”或者“内部指令”；
- -p：如果给出的指令为外部指令，则显示其绝对路径；
- -a：在环境变量“PATH”指定的路径中，显示给定指令的信息，包括命令别名。
范例：  
```
type -p java
```
输出：/bin/java

# 4. 查看当前路径
```
pwd
```

# 5. 历史执行过的命令
```
history
```

