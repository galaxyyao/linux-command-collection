# Linux用户与授权
## 1. 用户
**创建用户**  
```shell
useradd 用户名
```

**创建禁止登录的用户（服务启用账号）**  
```shell
useradd -r -g mysql -s /bin/false mysql
```
解释：  
- -r：建立系统账号
- -g：指定用户所属的起始群组
- -s：指定用户登录后所使用的shell

**创建特殊组(root)的用户**  
```
useradd -u 0 -g 0 用户名
```

**修改用户密码**  
```shell
passwd 用户名
```

**查看密码过期时间**  
```shell
chage -l 用户名
```

**设置密码永不过期**  
修改/etc/login.defs里的PASS_MAX_DAYS，改为99999  
或  
```shell
chage -I -1 -m 0 -M 99999 -E -1 用户名
```

**删除用户**  
```
userdel <用户>
```

**列出所有用户**  
```
compgen -u
```
或
```
cut -d: -f1 /etc/passwd
```

**查看当前已登录用户**  
```
w
```

## 2. 组
**列出所有组**  
```
compgen -g
```

**查看某个组里的用户**  
```
lid -g 组名
```

## 3. 授权
**修改已有用户权限**  
```
vi /etc/passwd
```

**改变目录的所有人**  
```
chown 用户:群组 目录
```
**改变目录的所有人（包含子目录）**  
```
chown -R 用户:群组 目录
```

## 4. 切换用户
**切换其他用户身份**  
```
su - <用户>
```

以某个用户权限执行命令
```
runuser -l <用户名> -c '命令'
```
