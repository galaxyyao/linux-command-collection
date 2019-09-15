# Linux Shell脚本调用
## 1. 解释器程序路径
在脚本开头加上
```
#!/bin/sh
```
sh相比bash，表示增加了遵循POSIX规范

## 2. 三种调用方式fork/source/exec
### 2.1 fork方式
当前进程创建一个子进程
```
./test.sh
```

### 2.2 source方式
source与"."等价  
不另外创建子进程，在当前shell环境中执行  
```
source ./test.sh
```

### 2.3 exec方式
不另外创建子进程，但是会终止当前的shell执行  
```
exec ./test.sh
```

## 2. bash参数
**语法检查**  
```
bash -n myscript.sh
```
**跟踪命令后执行**  
```
bash -v myscript.sh
```
**附加扩展信息**  
```
bash -x myscript.sh
```

## 3. 后台执行
当用户注销（logout）或者网络断开时，终端会收到 HUP（hangup）信号从而关闭其所有子进程。  
**参考资料**  
https://unix.stackexchange.com/questions/3886/difference-between-nohup-disown-and  
https://stackoverflow.com/questions/10408816/how-do-i-use-the-nohup-command-without-getting-nohup-out  

如果需要在后台执行，可以在命令最后加一个`&`，例如：  
```
./xxx.sh &
```
**后台执行并忽略HUP信号**  
```
nohup xxx.sh &
```
**后台执行并忽略HUP信号，且不输出nohup.out**  
```
nohup command >/dev/null 2>&1   # doesn't create nohup.out
nohup sh run.sh &>/dev/null &
```

## 4. 查看当前作业
```
jobs
```
**使已在运行的作业忽略hup信号**  
```
disown -h jobspec 使某个作业忽略HUP信号
disown -ah 使所有的作业都忽略HUP信号
disown -rh 使正在运行的作业忽略HUP信号
```

## 5. 脚本接受启动参数
在脚本中使用`$+数字`  
范例：  
```shell
$ cat myscript.sh
#!/bin/bash
echo "First arg: $1"
echo "Second arg: $2"

$ ./myscript.sh hello world
First arg: hello
Second arg: world
```




