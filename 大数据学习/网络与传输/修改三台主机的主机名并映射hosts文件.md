# 修改三台主机的主机名并映射hosts文件

## 1.修改三台主机的主机名(方便映射hosts文件)

```bash
vim /etc/hostname
```

![修改hostname文件过程.png](修改hostname文件过程.png)

![image.png](修改hostname文件过程2.png)

![image.png](修改hostname文件过程3.png)

## 2.主机名映射hosts文件

```bash
vim /etc/hosts

对应主机ip hadoop102
对应主机ip hadoop101
对应主机ip hadoop103

```

## 若使用的为云服务器则按照如下操作

[使用云服务器配置hosts文件后 重启之后失效解决](使用云服务器配置hosts文件后%20重启之后失效解决.md) 

[使用云服务器修改主机名称](使用云服务器修改主机名称.md)