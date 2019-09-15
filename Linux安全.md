# Linux安全
## 1. 端口
**查看占用端口的进程**  
```
netstat -tulpn | grep :端口号
```

## 2. 防火墙
### 2.1 防火墙端口
**获取active zone**  
```
firewall-cmd --get-active-zones
```
**打开单个防火墙端口**  
```
firewall-cmd --zone=public --add-port=端口号/tcp --permanent
firewall-cmd --reload
```
**打开一个范围的端口**  
```
sudo firewall-cmd --zone=public --permanent --add-port=4990-4999/udp
firewall-cmd --reload
```
**打开http服务相关端口（Nginx适用）**  
```
sudo firewall-cmd --zone=public --permanent --add-service=http
firewall-cmd --reload
```
**移除已打开的防火墙端口**  
```
firewall-cmd --zone=public --remove-port=80/tcp --permanent
firewall-cmd --reload
```
**列出已开放的端口**  
```
firewall-cmd --zone=public --list-ports
```
**列出防火墙开放的服务**  
```
firewall-cmd --list-service
```

### 2.2 防火墙服务
**重新加载防火墙服务**  
```
firewall-cmd --reload
```
**启动防火墙服务**  
```
systemctl start firewalld.service
```
**设置防火墙开机启动**  
```
systemctl enable firewalld.service
```
**停止防火墙服务**  
```
systemctl stop firewalld.service
```
**禁止防火墙开机启动**  
```
systemctl disable firewalld.service
```
**查看防火墙服务状态**  
```
systemctl status firewalld.service
```

### 2.3 iptables
仅对于CentOS 6及以下或RHEL适用  
**关闭防火墙**  
```
service iptables stop
```
**永久关闭防火墙**  
```
chkconfig iptables off
```
**开启端口**  
```
iptables -I INPUT -p tcp --dport 8080 -j ACCEPT
service iptables save
```
**查看iptables开放的端口和其他规则**  
```
service iptables status
```
**删除某条规则**  
```
iptables -D INPUT 数字
```

# 3. SELinux
参考资料：https://wiki.centos.org/zh/HowTos/SELinux  
**查看SELinux状态**  
```
sestatus
```
**关闭selinux**  
`vi /etc/selinux/config`，修改`SELINUX=disabled`，保存后重启服务器

# 4. OPENSSL
**生成自定义密钥对**  
```
openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout tls.key -out tls.crt -subj "/CN=traefik-ui.anxintrust.net"
```
**cer转crt**  
```
openssl x509 -inform PEM -in tls.cer -out tls.crt
openssl x509 -inform DER -in tls.cer -out tls.crt
```
**cer转pem**  
```
openssl x509 -inform DER -in certificate.cer -out certificate.pem
```

# 5. 系统安全工具
## 5.1 Fail2ban
防止SSH暴破和端口扫描  
参考资料：  
[系统运维|如何使用 fail2ban 防御 SSH 服务器的暴力破解攻击](https://linux.cn/article-5067-1.html)  
[系统运维|如何在 Linux 上用 Fail2Ban 保护服务器免受暴力攻击](https://linux.cn/article-9299-1.html)  


