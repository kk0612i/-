# 初次使用Hadoop统计单词出现次数(小Demo)

## 1.进入Hadoop目录并创建 input 文件夹存放需要统计的文件

> 这样做只是为了这个案列更加方便 创建完input目录后进入创建需要统计的测试文件
> 

```bash
mkdir input
cd input
```

> 创建多个测试文件并编辑内容
> 

```bash
touch test1.txt test2.txt test3.txt
vim test1.txt
vim test2.txt
vim test3.txt
```

## 2.在Hadoop根目录下执行Hadoop自带的案例

> **需要注意**的是：**output文件夹不能存在！！！**
> 

```bash
 hadoop jar ./share/hadoop/mapreduce/hadoop-mapreduce-examples-3.1.3.jar wordcount ./input output
```

### 进入output文件夹中

![任务执行成功文件.png](任务执行成功文件.png)

> 当看到 **_SUCCESS时证明该案列执行成功**
> 

## 3.查看Hadoop统计的结果

> 使用 **cat 命令** 查看Hadoop生成的part-r文件
> 

![image.png](查看统计单词统计结果内容.png)

## 4.尝鲜Demo完成！