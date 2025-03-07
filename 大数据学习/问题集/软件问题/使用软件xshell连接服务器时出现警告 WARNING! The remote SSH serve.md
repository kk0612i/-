# 使用软件xshell连接服务器时出现警告 :WARNING! The remote SSH server rejected X11 forwarding request.

# 警告信息:`WARNING! The remote SSH server rejected X11 forwarding request.` (如图)

![软件警告内容.png](软件警告内容.png)

> 此警告信息意思为:远程ssh服务器拒绝x11转发请求
> 

> 虽然只是警告信息 对**命令的正常使用**无伤大雅 但看到警告还是想要消除的
> 

# 解决办法

## 1.(推荐该方法) **X11 forwarding依赖xorg-x11-xauth软件包 需要先安装xorg-x11-xauth软件包**

> 命令:
> 

```bash
yum install -y xorg-x11-xauth
```

如图：

![image.png](安装依赖包命令.png)

**安装成功后重新连接！查看是否解决**

> 如图顺利解决 连接不再出现警告信息
> 

![image.png](成功去除警告.png)