# 使用云服务器配置hosts文件后 重启之后失效解决

> 当我们使用 `vim /etc/hosts` 命令修改**云服务器hosts**文件后 重启之后并发现一切恢复如初
> 

> `vim`修改成功后：（如图）
> 

![云服务器上配置hosts文件内容.png](云服务器上配置hosts文件内容.png)

> 重启重新连接后再次查看`hosts` 文件
> 

![image.png](重启后使用cat命令再次查看hosts文件内容.png)

> 此时我们可以看到一切恢复如初
> 

# 解决办法

## 修改`hosts.redhat.tmpl` 文件

```bash
vim /etc/cloud/templates/hosts.redhat.tmpl 
```

如图：

![image.png](云服务器上正确修改hosts文件位置.png)

如图：

![image.png](云服务器上正确修改hosts文件内容.png)

> 再次重启查看hosts文件
> 

![image.png](cat命令查看hosts文件内容.png)

> **成功！所以当我们使用云服务器时需要配置hosts映射文件时应该去配置**`/etc/cloud/templates/` 下的`hosts.redhat.tmpl`  文件
>