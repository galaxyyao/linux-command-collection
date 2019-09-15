# Linux网络
## 1. 网络相关配置
## 1.1 MAC地址
**查询MAC地址**  
ifconfig -a

## 1.2 IP
**查询本机IP**  
```shell
/sbin/ifconfig -a | grep inet | grep -v 127.0.0.1 | grep -v inet6 | grep -v 192.* | awk '{print $2}' | tr -d "addrs"  
```

**查询本机在外网的IP**  
```
curl ipinfo.io/json
curl ipinfo.io/ip
curl ip.cn
curl cip.cc
curl myip.ipip.net
```

## 1.3 主机名Hostname
**查询主机名**  
```
hostnamectl
```

**修改host名**  
参考资料：https://www.howtogeek.com/50631/how-to-change-your-linux-hostname-without-rebooting/  
```
vi /etc/sysconfig/network
vi /etc/hosts
hostname XXXX
service network restart
/etc/init.d/network restart
```
或
```
hostnamectl set-hostname XXXX
```

## 1.4 DNS
### 查询DNS
```
nslookup 域名
```
或
```
dig 域名
```
范例：`dig @127.0.0.1 -p 8600 web.service.consul`  

### 修改DNS
#### 方法一：修改网络接口配置文件
```
cd /etc/sysconfig/network-scripts/
```
修改配置文件，XXX根据不同的服务器而不同：  
```
vi ifcfg-XXX
```
添加DNS解析地址：  
```
DNS1=X.X.X.X
DNS2=114.114.114.114
```
然后重启network服务：  
```
systemctl restart network.service
```

#### 方法二：修改resolv.connf
```
vi /etc/resolv.conf
```
添加nameserver：  
```
nameserver 10.1.2.88
nameserver 114.114.114.114
```
然后重启NetworkManager服务：  
```
systemctl restart NetworkManager.service
```

## 2. 网络相关工具
### 2.1 wget
```
wget 下载链接
```

### 2.2 curl
参考资料：https://curl.haxx.se/docs/manual.html  

**GET方式**  
范例：  
```
curl http://localhost:9999/getrootdir
curl -H "content-type: application/json"  -H "Authorization: Basic YWRtaW46YWRtaW4="  http://ip:15672/api/overview
```

**POST方式**  
范例：  
```
curl --request POST   --url http://127.0.0.1:14000/rest/push   --header 'cache-control: no-cache'   --header 'content-type: application/json'   --header 'is_sync: 1'   --header 'push_method: push_method_all'   --header 'receive_mode: http'   --header 'system: test'   --header 'ticket: test'   --header 'time: 20170620164000'   --data '{"serial_no":1, "app_code":"homeappuat", "content":"测试所有"}'
```

**排查CORS跨域问题**  
参考资料：https://stackoverflow.com/questions/12173990/how-can-you-debug-a-cors-request-with-curl  
```
curl -H "Origin: http://example.com" -H "Access-Control-Request-Method: POST"   -H "Access-Control-Request-Headers: X-Requested-With"  -X OPTIONS --verbose https://address
```
测试CORS站点：https://www.test-cors.org  


### 2.3 SSH
```
ssh -l <用户名> <服务器>
```
**SSH互信**
参考资料：https://blog.csdn.net/kongqz/article/details/6338690  

**root用户可SSH登录配置**  
```
sed -i '/^#PermitRootLogin/s/#PermitRootLogin yes/PermitRootLogin yes/' /etc/ssh/sshd_config
cat /etc/ssh/sshd_config|grep RootLogin
```
预期返回`PermitRootLogin yes`

参考资料：[SSH登陆问题及排查思路](https://www.infoq.cn/article/pqU7iMf8cHpz-RNLOslJ)

### 2.4 NMAP
网络映射器，用于网络发现和安全审计的网络安全工具
```
nmap -sP 192.168.0.0/24   判断哪些主机存活
nmap -sT 192.168.0.3   开放了哪些端口
nmap -sS 192.168.0.127 开放了哪些端口（隐蔽扫描）
nmap -sU 192.168.0.127 开放了哪些端口（UDP）
nmap -sS -O  192.168.0.127 操作系统识别
nmap -sT -p 80 -oG – 192.168.1.* | grep open    列出开放了指定端口的主机列表
nmap -sV -p 80 thief.one  列出服务器类型(列出操作系统，开发端口，服务器类型,网站脚本类型等)
```