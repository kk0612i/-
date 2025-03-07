# 创建Hadoop学习途中使用的用户并设置文件夹归属

## 1.创建一个新用户并设置密码

> 创建用户命令：**useradd $username**
> 
> 
> 设置用户的密码命令: **passwd $username**
> 

示例：

```bash
useradd kk
passwd kk
```

![创建新用户过程.png](创建新用户过程.png)

> 就此即**完成**了创建新用户的功能
> 

## 2.配置用户具有root权限，方便后面加sudo命令执行root权限命令

```bash
vim /etc/sudoers
```

进入vim之后 **输入 /root** 进行搜索 按 **n 进行下一个查找 直到找到如下代码块这一块**

```bash
## Allow root to run any commands anywhere 
root    ALL=(ALL)       ALL
```

找到之后在下方插入变更为如下信息

```bash
## Allow root to run any commands anywhere 
root    ALL=(ALL)       ALL
kk      ALL=(ALL)       NOPASSWD:ALL
```

完成之后因为该文件为只读文件 我们按 **esc 输入:wq! 强制执行写入并退出**

## 3.设置 software和 opt 目录的归属用户和归属组

> 设置文件夹归属用户与归属组命令
> 

```bash
chown kk:kk /opt/module
chown kk:kk /opt/software
```

## 4.切换为刚刚创建的kk用户

```bash
su kk
```

![image.png](切换用户命令.png)

**~~完成！！！~~**