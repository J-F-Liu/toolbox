# CoreOS

查看当前系统版本
```
$ cat /etc/os-release
```

## 系统升级
官方提供三个升级通道：Alpha（内测版）、Beta（公测版 ）和 Stable（正式发行版）。

各通道发布更新的频率依次为（见官方博客声明）：
- Alpha：每周星期四发布
- Beta：每两周发布一次
- Stable：每个月发布一次

每个通道当前的系统版本号及内置组件版本号可以在[这个网页](https://coreos.com/releases/)上查看到。

升级策略主要与自动升级后的重启更新方式有关。它的值可以是：
- best-effort：如果Etcd运行正常则相当于etcd-lock，否则相当于reboot，为默认值。
- etcd-lock：自动升级后自动重启，使用LockSmith服务调度重启过程
- reboot：自动升级后立即自动重启系统
- off：自动升级后等待用户手工重启

LockSmith服务用于防止过多的节点同时重启导致对外服务中断和Etcd的Leader节点选举无法进行。通过设定固定数量的锁，只有获得锁的主机才能够进行重启升级，否则就继续监听锁的变化。重启升级后的节点会释放它占用的锁，从而通知其他节点开始下一轮获取升级锁的竞争。

查看升级锁的状态信息：
```
$ locksmithctl status
Available: 0     <-- 剩余的锁数量
Max: 1           <-- 锁的总数
MACHINE ID
010a2e41e747415ba51212fa995801dd  <-- 获得锁的节点
```

可用`locksmithctl set-max`命令可用修改升级锁数量（即允许同时重启升级的节点数量）。

在/etc/coreos/update.conf中设置升级参数：
```
GROUP=Stable
REBOOT_STRATEGY=off
SERVER=https://example.update.core-os.net
```
每次修改完成后执行`sudo systemctl restart update-engine`命令使配置生效。

CoreOS始终会自动在后台下载和部署新版本系统，即使将升级策略设为off（这样只是禁止自动重启）。CoreOS会在启动后10分钟以及之后的每隔1个小时自动检测系统版本，具体的升级检测记录可以通过`journalctl -f -u update-engine`命令查看到。

手动触发升级
```
update_engine_client -update
```

使用HTTP代理升级CoreOS
```
nano /etc/systemd/system/update-engine.service.d/proxy.conf
```
```
[Service]
Environment=ALL_PROXY=http://your.proxy.address:port
```

将ALL_PROXY的值换成实际的代理服务器地址，重启一下update-engine服务。

## 配置docker国内镜像
```
echo 'DOCKER_OPTS="--registry-mirror=http://aad0405c.m.daocloud.io"' > /etc/default/docker
systemctl restart docker
```

## 安装nano
```
mkdir -p /opt/bin
docker run -d --name arch base/archlinux:latest sleep
docker cp arch:/usr/bin/nano /opt/bin
docker rm arch
```
or in local computer run
```
scp /usr/bin/nano root@<server address>:/opt/bin
```

## 运行htop
```
alias htop='docker run --rm -it --pid host tehbilly/htop'
htop
```

## 开机时启用swap文件
```
fallocate -l 2G /swapfile
chmod 600 /swapfile
mkswap /swapfile
nano /etc/systemd/system/swapon.service
systemctl enable --runtime /etc/systemd/system/swapon.service
```
> ```
[Unit]
Description=Turn on swap
>
[Service]
Type=oneshot
ExecStart=/sbin/swapon /swapfile
>
[Install]
WantedBy=local.target
```

## 申请网站SSL证书
```
docker run -it \
           --rm \
           --net host \
            -v /etc/letsencrypt:/etc/letsencrypt \
            -v /var/lib/letsencrypt:/var/lib/letsencrypt \
            gzm55/certbot certonly --standalone --text -d www.yunluwang.com "$@"
```

## etcd
etcd is a distributed, consistent key-value store, written in Go.

Using the Raft consensus algorithms, etcd gracefully handles network partitions and machine failures, even leader failures.
etcd clusters are based on a strong leader. A leader is elected by other members in cluster. Once elected, the leader starts processing client requests and replicating them to its followers. All server-to-server communication is done by RPC (Remote Procedure Call).

## fleet
fleet is a cluster manager that controls systemd at the cluster level. To run your services in the cluster, you must submit regular systemd units combined with a few fleet-specific properties.

Two types of units can be run in your cluster — standard and global units.
- Standard units are long-running processes that are scheduled onto a single machine. If that machine goes offline, the unit will be migrated onto a new machine and started.
- Global units will be run on all machines in the cluster.
