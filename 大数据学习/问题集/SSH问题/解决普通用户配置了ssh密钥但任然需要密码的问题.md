# 解决普通用户配置了ssh密钥但任然需要密码的问题

# 1.监控日志文件查看错误

> 监控命令：
> 

```bash
tail -f /var/log/secure
```

> 日志内容：
> 

```bash
Mar  2 14:50:08 hadoop102 sshd[22148]: pam_unix(sshd:auth): check pass; user unknown
Mar  2 14:50:08 hadoop102 sshd[22148]: pam_unix(sshd:auth): authentication failure; logname= uid=0 euid=0 tty=ssh ruser= rhost=45.159.112.96
Mar  2 14:50:10 hadoop102 sshd[22148]: Failed password for invalid user user from 45.159.112.96 port 44088 ssh2
Mar  2 14:50:12 hadoop102 sshd[22148]: Connection closed by 45.159.112.96 port 44088 [preauth]
Mar  2 14:50:13 hadoop102 sshd[22150]: Invalid user ftpuser from 45.159.112.96 port 44102
Mar  2 14:50:13 hadoop102 sshd[22150]: input_userauth_request: invalid user ftpuser [preauth]
Mar  2 14:50:14 hadoop102 sshd[22150]: pam_unix(sshd:auth): check pass; user unknown
Mar  2 14:50:14 hadoop102 sshd[22150]: pam_unix(sshd:auth): authentication failure; logname= uid=0 euid=0 tty=ssh ruser= rhost=45.159.112.96
Mar  2 14:50:14 hadoop102 sudo:      kk : TTY=pts/0 ; PWD=/ ; USER=root ; COMMAND=/bin/tail -f /var/log/secure
Mar  2 14:50:14 hadoop102 sudo: pam_unix(sudo:session): session opened for user root by root(uid=0)
Mar  2 14:50:16 hadoop102 sshd[22150]: Failed password for invalid user ftpuser from 45.159.112.96 port 44102 ssh2
Mar  2 14:50:16 hadoop102 sshd[22150]: Connection closed by 45.159.112.96 port 44102 [preauth]
Mar  2 14:50:17 hadoop102 sshd[22152]: pam_unix(sshd:auth): authentication failure; logname= uid=0 euid=0 tty=ssh ruser= rhost=45.159.112.96  user=root
Mar  2 14:50:17 hadoop102 sshd[22152]: pam_succeed_if(sshd:auth): requirement "uid >= 1000" not met by user "root"
Mar  2 14:50:17 hadoop102 sshd[22156]: pam_unix(sshd:auth): authentication failure; logname= uid=0 euid=0 tty=ssh ruser= rhost=218.92.0.247  user=root
Mar  2 14:50:17 hadoop102 sshd[22156]: pam_succeed_if(sshd:auth): requirement "uid >= 1000" not met by user "root"
Mar  2 14:50:18 hadoop102 sshd[22152]: Failed password for root from 45.159.112.96 port 52258 ssh2
**Mar  2 14:50:19 hadoop102 sshd[22161]: Authentication refused: bad ownership or modes for directory /home/kk**
Mar  2 14:50:19 hadoop102 sshd[22152]: Connection closed by 45.159.112.96 port 52258 [preauth]
Mar  2 14:50:19 hadoop102 sshd[22156]: Failed password for root from 218.92.0.247 port 49779 ssh2
Mar  2 14:50:19 hadoop102 sshd[22156]: pam_succeed_if(sshd:auth): requirement "uid >= 1000" not met by user "root"
Mar  2 14:50:20 hadoop102 sshd[22158]: Invalid user amir from 45.159.112.96 port 52274
Mar  2 14:50:20 hadoop102 sshd[22158]: input_userauth_request: invalid user amir [preauth]
Mar  2 14:50:21 hadoop102 sshd[22156]: Failed password for root from 218.92.0.247 port 49779 ssh2
Mar  2 14:50:21 hadoop102 sshd[22156]: pam_succeed_if(sshd:auth): requirement "uid >= 1000" not met by user "root"
Mar  2 14:50:21 hadoop102 sshd[22158]: pam_unix(sshd:auth): check pass; user unknown
Mar  2 14:50:21 hadoop102 sshd[22158]: pam_unix(sshd:auth): authentication failure; logname= uid=0 euid=0 tty=ssh ruser= rhost=45.159.112.96
Mar  2 14:50:23 hadoop102 sshd[22156]: Failed password for root from 218.92.0.247 port 49779 ssh2
Mar  2 14:50:23 hadoop102 sshd[22158]: Failed password for invalid user amir from 45.159.112.96 port 52274 ssh2
Mar  2 14:50:23 hadoop102 sshd[22156]: Received disconnect from 218.92.0.247 port 49779:11:  [preauth]
```

> 查找关键信息：**`Mar  2 14:50:19 hadoop102 sshd[22161]: Authentication refused: bad ownership or modes for directory /home/kk`**
> 

> 这表明**SSH服务拒绝了认证请求**，原因是`/home/kk`目录的所有权或权限设置不正确。SSH对用户主目录及其子目录的权限有严格的要求
> 

> 要求如下：
> 
- 用户主目录（如`/home/kk`）的权限必须是755（即`drwxr-xr-x` ）
- `.ssh` 目录的权限必须是700（即`drwx------` ）
- `authorized_keys`文件的权限必须是`600`（即`-rw-------` ）

# 2.解决办法

> 修正权限：
> 

```bash
chmod 755 /home/kk
chmod 700 /home/kk/.ssh
chmod 600 /home/kk/.ssh/authorized_keys
chown -R kk:kk /home/kk
```

# 3. 测试连接

```bash
ssh xxxx
```