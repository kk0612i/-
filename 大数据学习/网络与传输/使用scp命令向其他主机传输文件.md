# 使用scp命令向其他主机传输文件

## 1.为了方便操作请先完成主机名对hosts文件的映射操作

[修改三台主机的主机名并映射hosts文件](修改三台主机的主机名并映射hosts文件.md) 

## 2.scp命令的使用

```bash
scp -r $pdir/fname $user@host:$pdir/fname
```

命令介绍：

| `scp` | `-r` | `$pdir/fname` | `$user@host:$pdir/fname` |
| --- | --- | --- | --- |
| 命令 | 递归 | 要拷贝的文件路径和文件名(**`./` 代表当前文件夹下的所有文件)** | 目标用户@主机:目的地路径 |

如图：

![scp命令传输过程.png](./Assets/scp命令传输过程.png)