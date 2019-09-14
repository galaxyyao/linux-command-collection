# Linux性能
## 1. sar
### 1.1 怀疑CPU瓶颈
CPU使用率
```
sar -u
```
平均负载
```
sar -q
sar -q 1（每秒统计一次）
```
load average(ldavg-n，n为n分钟内平均）超过5算比较高的了。

### 1.2 怀疑内存瓶颈
内存使用
```
sar -r
```
页面交换
```
sar -W
sar -B
```

### 1.3 怀疑I/O瓶颈
```
sar -b
sar -u
```
磁盘IO
```
sar -d
```
## 2. 收集系统信息
### 2.1 CPU
cat /proc/cpuinfo

### 2.2 内存
**free**  
```
free -m
```
**proc**  
```
cat /proc/meminfo
```
**vmstat**  
```
vmstat -s
```
**top**  
每列解释：
- us — 用户空间占用CPU的百分比。
- sy — 内核空间占用CPU的百分比。
- ni — 改变过优先级的进程占用CPU的百分比
- id — 空闲CPU百分比
- wa — IO等待占用CPU的百分比
- hi — 硬中断（Hardware IRQ）占用CPU的百分比
- si — 软中断（Software Interrupts）占用CPU的百分比
```
htop
```

## 硬盘
```
sudo fdisk -l
hdparm -i /dev/device (for example sda1, hda3...)
```

#### 参考资料
[工具参考篇 — Linux Tools Quick Tutorial](https://linuxtools-rst.readthedocs.io/zh_CN/latest/tool/index.html)
