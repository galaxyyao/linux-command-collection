# Linux安装以及Java相关
## 1. yum安装依赖包
```
yum install 包名
```
**yum静默安装**  
```
yum install -y 名称
```
**yum只下载不安装**  
```
yum install --downloadonly --downloaddir=/tmp 包名
```
**yum搜索包**  
```
yum search 关键字
```
**yum移除包**  
```
yum remove 包名
```

### 1.1 搭建私有YUM仓库
**仓库端配置**  
```
# 1. 创建仓库存放路径
mkdir -p /yumrepo
# 2. 安装必要依赖
yum -y install createrepo yum-utils
# 3. 初始化 repodata 索引文件
createrepo -pdo /yumrepo/ /yumrepo/
# 4. 提供http服务
cd /yumrepo/
python -m SimpleHTTPServer 80
# 5. 下载包并更新（以wget为例）
yumdownloader wget
createrepo --update /yumrepo/
```

**客户端配置**  
```
# 1. 配置私有源
cd /etc/yum.repos.d
# 2. 指定私有仓库源
vi private.repo
# 添加内容
[private]
name=private
baseurl=http://ip
enable=1  #1表示启用，没有此参数也表示启用
gpgcheck=0
```

**启动私有仓库**  
```
yum –-enablerepo=private –-disablerepo=base,extras,updates,epel list
或vi CentOS-Base.repo
# 在每一个启动的源加上
# enabled=0
```
在每次yum源更新包之后，都需要在客户端执行:  
```
yum clean all
yum makecache
```

## 2. RPM安装
手动下载RPM包：https://rpmfind.net/linux/rpm2html/search.php  
rpm安装命令：  
```
rpm -ivh RPM包
```
常用参数解释：  
- -i：install
- -v：详细输出
- -h：print hash marks as package installs (good with -v)

## 3. 常用软件的安装方式
### 3.1 SFTP
安装SFTP服务器：https://www.howtoforge.com/tutorial/how-to-setup-an-sftp-server-on-centos/  

### 3.2 jdk
```
yum search java | grep 'java-'
yum install java-1.8.0-openjdk.x86_64
# yum install java-devel
```

### 3.3 maven
```
wget http://repos.fedorapeople.org/repos/dchen/apache-maven/epel-apache-maven.repo -O /etc/yum.repos.d/epel-apache-maven.repo
yum -y install apache-maven
```
- 全局配置目录：/usr/share/apache-maven/conf/settings.xml
- 用户配置目录：/root/.m2/settings.xml
- 本地仓库路径：/root/.m2/repository

### 3.4 tomcat
参考资料：https://www.digitalocean.com/community/tutorials/how-to-install-apache-tomcat-7-on-centos-7-via-yum  
```
sudo yum install tomcat
```
**安装admin包**  
```
sudo yum install tomcat-webapps tomcat-admin-webapps
```
- 配置文件：/usr/share/tomcat/conf/tomcat.conf
- 应用目录：/usr/share/tomcat
- conf目录：/usr/share/tomcat/conf或/etc/tomcat
- logs目录：/var/log/tomcat/
- webapps目录：/usr/share/tomcat/webapps或/var/lib/tomcat/webapps

**修改JAVA_OPTS配置**  
```
sudo vi /usr/share/tomcat/conf/tomcat.conf
```
在最后添加
```
JAVA_OPTS="-Djava.security.egd=file:/dev/./urandom -Djava.awt.headless=true -Xmx4G -XX:MaxPermSize=256m -XX:+UseConcMarkSweepGC"
```

**修改server.xml里编码配置**  
```
vi /usr/share/tomcat/conf/server.xml
```
```xml
    <Connector port="8080" protocol="HTTP/1.1"
               connectionTimeout="20000"
               redirectPort="8443" URIEncoding="UTF-8" />
```

**tomcat服务启停**  
```
systemctl start tomcat
systemctl restart tomcat
systemctl enable tomcat
```

## 4. Java相关
**获取java版本**  
```
readlink -f $(which java)
```

**切换jdk**  
```
alternatives --config java
```
然后选择想切换的jdk版本  

**执行可运行jar包**  
```
java -jar XXXX.jar
```
**使用特定版本JDK执行jar包**  
```
/usr/java/jdk1.8.0_121/bin/java -jar XXXX.jar
```

### 4.1 java/tomcat性能排查
**DUMP命令**  
```shell
jmap -F -dump:format=b,file=heap.bin <pid>
```

**自动化收集jstat和jmap脚本**  
```shell
#----------Define-------------
pid=$(ps -ef | grep tomcat | grep -v 'grep' | awk '{print $2}')

#----------Main---------------
echo `date "+%Y-%m-%d %H:%M:%S"`
echo "----------"`date "+%Y-%m-%d %H:%M:%S"`"----------"
echo "jstat:"
jstat -gcutil ${pid}
echo " "
echo "jmap:"
jmap -heap ${pid}
echo "----------"`date "+%Y-%m-%d %H:%M:%S"`"----------"
echo " "
```

## 常见问题
### GCC更新
需要提前确认`bzip2`是否存在。如果不存在的话需要先`yum install bzip2`
安装GCC shell脚本（install-gcc-4.9.3.sh）：  
https://gist.github.com/jtilly/2827af06e331e8e6b53c  
https://gist.github.com/jeetsukumaran/5224956  
如果make过程中报错，可以执行：  
```
yum install glibc-headers
yum install gcc-c++
```