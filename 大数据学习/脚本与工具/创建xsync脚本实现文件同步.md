# 创建xsync脚本实现文件同步

# 0.完成ssh密钥配置

[生成三台主机的ssh-key并配置](生成三台主机的ssh-key并配置.md) 

# 1.进入家目录创建 `bin` 文件存放脚本文件

```bash
cd ~
mkdir bin
```

# 2.创建xsync脚本并实现文件同步功能

```bash
vim xsync
```

> **功能代码**如下：
> 

```bash
#!/bin/bash
#1.判断参数个数
if[ $# -lt 1 ]
then
	echo "未传入参数"
	exit
fi

#2.遍历集群机器
for host in hadoop102 hadoop101 hadoop103
do
	echo ============================= $host ==================================
	#3.遍历所有目录 一个个发送
	for file in $@
	do
		#4.判断文件是否已经存在
		if [ -e $file ]
		then
			#5.获取父目录
			pdir=$(cd -P $(dirname $file); pwd)

			#6.获取当前文件的名称
			fname=$(basename $file)
			ssh $host "mkdir -p $pdir"
			rsync -av $pdir/$fname $host:$pdir
		else
			echo $file does not exists!
		fi
	done
done

```

# 3.给予脚本执行权限

```bash
chmod +x xsync
```

如图：

![给予脚本可执行权限.png](给予脚本可执行权限.png)

# 4.加入环境变量以便全局使用

> 编辑之前设置环境变量的脚本 `my_env.sh`
> 

![image.png](修改自定义环境变量脚本的命令.png)

> 添加如下内容
> 

![image.png](修改变化的内容.png)

```bash
#JAVA_HOME
export JAVA_HOME=/opt/module/jdk1.8.0_212
#HADOOP_HOME
export HADOOP_HOME=//opt/module/hadoop-3.1.3
#~/bin
export HOME_BIN=~/bin
#将JAVA_HOME添加到环境变量中
export PATH=$PATH:$JAVA_HOME/bin:$HADOOP_HOME/bin:$HADOOP_HOME/sbin:$HOME_BIN
```

> 刷新环境变量
> 

```bash
source /etc/profile
```

# 5.尝试是否配置成功

> 如图**成功配置完成  就此`~/bin` 目录下的脚本全局可使用**
> 

![image.png](检查环境变量是成功生效.png)