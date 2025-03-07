# ssh配置完密钥后任然需要密码

> 如图所示
> 

![授权公钥却任然需要密码.png](授权公钥却任然需要密码.png)

# 解决办法：

> 修改`sshd_config` 文件
> 

```bash
vim /etc/ssh/sshd_config
```

将`PubkeyAuthentication` 参数值改为`yes` （如图）

![image.png](启用公钥验证参数.png)

> 此参数意思为是否启用公钥验证 改为yes则启用
> 

> 修改完成后重启sshd服务
> 

```bash
service sshd restart
```

> 重启完成之后 再次使用`ssh` 命令连接尝试是否还需要密码（如图）
> 

![image.png](重启ssh服务命令并重新使用ssh连接.png)

> 就此解决了配置完密钥后还需要输入的问题 原因就是未启用公钥验证 只需要把**`PubkeyAuthentication`** 参数值从 **`no`** 修改为**`yes`** 即可
>