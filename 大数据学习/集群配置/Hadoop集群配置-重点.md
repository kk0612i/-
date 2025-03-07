# Hadoop集群配置-重点

# 1.集群规划

## **注意事项：**

> **`NameNode`** 和**`SecondaryNameNode`** 不要安装在同一台服务器上
> 

> **`ResourceManager`** 也很消耗内存 不要与**`NameNode`** 和 **`SecondaryNameNode`** 配置在同一台服务器上
> 

|      | hadoop101             | hadoop102                       | hadoop103                      |
| ---- | --------------------- | ------------------------------- | ------------------------------ |
| HDFS | `NameNode` `DataNode` | `DataNode`                      | `SecondaryNameNode` `DataNode` |
| YARN | `NodeManager`         | `ResourceManager` `NodeManager` | `NodeManager`                  |
|      |                       |                                 |                                |

# 2.配置文件说明

> Hadoop 配置文件分为**两类**：**默认配置文件**与**自定义配置文件** 只有用户想修改某一默认配置值时 才需要修改自定义配置文件 更改相应的属性值
> 

## 默认配置文件：

| 默认配置文件 | 文件存放于Hadoop的jar包中的位置 |
| --- | --- |
| `[core-delfault.xml]`  | `hadoop-common-3.1.3.jar/core-default.xml` |
| `[hdfs-default.xml]` | `hadoop-hdfs-3.1.3.jar/hdfs-default.xml`  |
| `[yarn-default.xml]` | `hadoop-yarn-common-3.1.3.jar/yarn-default.xml` |
| `[mapred-default.xml]` | `hadoop-mapreduce-client-core-3.1.3.jar/mapred-default.xml` |

## 自定义配置文件

> `core-site.xml`  `hdfs-site.xml`  `yarn-site.xml`  `mapred-site.xml` 这四个自定义配置文件存放于 `$HAOOP_HOME/etc/hadoop` 这个路径当中 可以根据自己的需求来修改配置
> 

## 总结：

> 默认的配置文件存放于jar包当中并不好更改 所以**给与用户自定义配置文件来根据自己的需求更改配置** Hadoop会**判断用户是否配置了自定义配置文件 优先读取自定义配置文件** 若没有配置自定义配置文件 则采用默认配置文件
> 

# 3.配置集群

## 0.进入存放Hadoop自定义配置文件目录

> 命令：

```bash
cd $HADOOP_HOME/etc/hadoop
```

如图：

![进入Hadoop配置文件目录.png](进入Hadoop配置文件目录.png)

## 1. 配置核心配置文件（`core-site.xml`）

> 命令：
> 

```bash
vim core-site.xml
```

> 文件内容：
> 

```xml
<?xml version="1.0" encoding="UTF-8"?>
<?xml-stylesheet type="text/xsl" href="configuration.xsl"?>

<configuration>
    <!-- 指定NameNode的地址 -->
    <property>
        <name>fs.defaultFS</name>
        <value>hdfs://hadoop101:8020</value>
    </property>

    <!-- 指定hadoop数据的存储目录 -->
    <property>
        <name>hadoop.tmp.dir</name>
        <value>/opt/module/hadoop-3.1.3/data</value>
    </property>

    <!-- 配置HDFS网页登录使用的静态用户为kk -->
    <property>
        <name>hadoop.http.staticuser.user</name>
        <value>kk</value>
    </property>
</configuration>

```

> 如图：
> 

![image.png](core-site.xml配置文件内容.png)

## 2.配置HDFS配置文件（`hdfs-site.xml`）

> 命令：
> 

```bash
vim hdfs-site.xml
```

> 文件内容：
> 

```xml
<?xml version="1.0" encoding="UTF-8"?>
<?xml-stylesheet type="text/xsl" href="configuration.xsl"?>

<configuration>
	<!-- nn web端访问地址-->
	<property>
        <name>dfs.namenode.http-address</name>
        <value>hadoop101:9870</value>
    </property>
	<!-- 2nn web端访问地址-->
    <property>
        <name>dfs.namenode.secondary.http-address</name>
        <value>hadoop103:9868</value>
    </property>
</configuration>

```

> 如图：
> 

![image.png](hdfs-site.xml配置文件内容.png)

## 3.配置YARN配置文件（`yarn-site.xml`）

> 命令：
> 

```bash
vim yarn-site.xml
```

> 文件内容：
> 

```xml
<?xml version="1.0" encoding="UTF-8"?>
<?xml-stylesheet type="text/xsl" href="configuration.xsl"?>

<configuration>
    <!-- 指定MR走shuffle -->
    <property>
        <name>yarn.nodemanager.aux-services</name>
        <value>mapreduce_shuffle</value>
    </property>

    <!-- 指定ResourceManager的地址-->
    <property>
        <name>yarn.resourcemanager.hostname</name>
        <value>hadoop102</value>
    </property>

    <!-- 环境变量的继承 -->
    <property>
        <name>yarn.nodemanager.env-whitelist</name>
        <value>JAVA_HOME,HADOOP_COMMON_HOME,HADOOP_HDFS_HOME,HADOOP_CONF_DIR,CLASSPATH_PREPEND_DISTCACHE,HADOOP_YARN_HOME,HADOOP_MAPRED_HOME</value>
    </property>
</configuration>

```

> 如图：
> 

![image.png](yarn-site.xml配置文件内容.png)

## 4.配置MapReduce配置文件（`mapred-site.xml`）

> 命令：
> 

```bash
vim mapred-site.xml
```

> 文件内容：
> 

```xml
<?xml version="1.0" encoding="UTF-8"?>
<?xml-stylesheet type="text/xsl" href="configuration.xsl"?>

<configuration>
	<!-- 指定MapReduce程序运行在Yarn上 -->
    <property>
        <name>mapreduce.framework.name</name>
        <value>yarn</value>
    </property>
</configuration>

```

> 如图：
> 

![image.png](mapred-site.xml配置文件内容.png)

# 4.使用xsync脚本分发配置好的Hadoop配置文件

> `xsync` 脚本编写页面：[7.创建xsync脚本实现文件同步](创建xsync脚本实现文件同步.md)
> 

> 命令：
> 

```bash
 xsync $HADOOP_HOME/etc/hadoop/
```

> 如图：
> 

![image.png](同步文件命令.png)

# 5.在各机器上检查是否成功分发配置

> 命令：
> 

```bash
[kk@hadoop101 hadoop-3.1.3]$ cat $HADOOP_HOME/etc/hadoop/core-site.xml 
[kk@hadoop103 /]$ cat $HADOOP_HOME/etc/hadoop/core-site.xml 
```

# 6.群起集群

## 1.配置`workers`

> 命令：
> 

```bash
vim $HADOOP_HOME/etc/hadoop/workers
```

> 配置内容：
> 

```bash
hadoop101
hadoop102
hadoop103
```

> **注意：该文件中添加的内容结尾不允许有空格 文件中不允许有空行**
> 

## 2.同步配置文件

> 命令：
> 

```bash
xsync $HADOOP_HOME/etc/
```

> 同步过程如下：
> 

```bash
[kk@hadoop102 /]$ xsync $HADOOP_HOME/etc
============================= hadoop102 ==================================
sending incremental file list

sent 908 bytes  received 19 bytes  1,854.00 bytes/sec
total size is 107,770  speedup is 116.26
============================= hadoop101 ==================================
sending incremental file list
etc/hadoop/
etc/hadoop/workers

sent 984 bytes  received 51 bytes  690.00 bytes/sec
total size is 107,770  speedup is 104.13
============================= hadoop103 ==================================
sending incremental file list
etc/hadoop/
etc/hadoop/workers

sent 988 bytes  received 51 bytes  2,078.00 bytes/sec
total size is 107,770  speedup is 103.72
[kk@hadoop102 /]$ 
```

## 3.启动集群

## 1.格式化`NameNode`

> 说明：
> 

> **若集群是第一次启动** 需要在**`hadoop101`节点格式化`NameNode`**（**注意：**格式化NameNode 会产生新的集群id 导致**`NameNode`**和**`DataNode`**的集群id不一致  集群找不到已往数据 如果集群在**运行过程中报错**  需要重新格式化**`NameNode`**的话 **一定要先停止`NameNode`**和**`DataNode`**进程，并且要删除所有机器的**`data`**和**`logs`**目录  然后再进行格式化
> 

> 代码：
> 

```bash
hdfs namenode -format
```

### 1.1启动`HDFS`

> 命令：
> 

```bash
[kk@hadoop101 hadoop-3.1.3]$ start-dfs.sh 
```

> 启动过程：
> 

```bash
[kk@hadoop101 hadoop-3.1.3]$ start-dfs.sh 
Starting namenodes on [hadoop101]
Starting datanodes
hadoop103: WARNING: /opt/module/hadoop-3.1.3/logs does not exist. Creating.
hadoop102: WARNING: /opt/module/hadoop-3.1.3/logs does not exist. Creating.
Starting secondary namenodes [hadoop103]
[kk@hadoop101 hadoop-3.1.3]$ 
```

## **2.在配置了`ResourceManager`的节点上**启动`YARN`