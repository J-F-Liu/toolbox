## rsync

https://rsync.samba.org/

rsync 支持增量的同步文件，它使用特有的“rsync 算法”计算文件的不同，只同步差异的部分，所以它的同步非常快。 rsync 支持远端的文件同步，使用原生的 rsync 传输协议，也可通过 SSH 协议传输，是 rcp 和 scp 理想的替代品。

```
pacman -S rsync
man rsync
rsync [OPTIONS] SRC DEST
```

例如 rsync -a --delete -P /mnt/backup/Model /data

```
-v --verbose 详细模式输出，传输时的进度等信息。
-z --compress 传输时进行压缩以提高传输效率,
--compress-level=NUM可按级别压缩
--delete 删除目标路径中额外的文件
-a --archive 归档模式，表示以递归方式传输文件，并保持所有文件属性，等价于-rtopgDl
-r 对子目录以递归模式，即目录下的所有目录都同样传输，注意是小写的r.
-o 保持文件属性信息
-p 保持文件权限
-g 保持文件属组信息
-P 显示同步的过程及传输时的进度等信息
-D 保持设备文件信息
-l 保持软连接
-avzP 提示：这里的 相当于 -vzrtopgDlP(还多了Dl功能)生产环境常用
-avz 定时任务就不用输出过程了可以-az
-e 使用的信道协议，指定替代rsh的shell程序，例如：ssh
--exclude=PATTERN 指定排除不需要传输的文件模式（和tar参数一样）--exclude=file（文件名所在的目录文件）（和tar参数一样）--delete 无差异同步，即全部同步。
```

rsync 可以分为 3 种传输数据的常用场景：

- 本地的文件复制
- 远端主机的文件传输，2 台主机必须都有 rsync 命令
- 作为 daemon 服务的方式提供服务，支持 rsync:// URL

非 daemon 模式下，连接时先给远程机发一个启动 rsync 进程的命令。

rsync daemon 模式下，支持用户名认证和读写的权限控制，默认的配置文件在 /etc/rsyncd.conf。用户的密码保存在/etc/rsyncd/rsyncd.pass。

```
systemctl start rsyncd
```
