## systemd

Unit是Systemd管理服务的基本单元，可以认为每个服务就是一个Unit，并使用一个Unit文件定义。Unit文件中需要包含相应服务的描述、属性以及需要运行的命令。

Target是Systemd中用于指定服务启动组的方式。每次系统启动的时候都会运行与当前系统相同级别Target关联的所有服务，如果服务不需要跟随系统自动启动，则完全可以忽略这个Target的内容。

### 服务管理

列出运行中的单元：
```
systemctl
```

列出运行失败的单元：
```
systemctl --failed
```

查看某个单元的状态：
```
systemctl status unit
```

立即启动某个单元：
```
systemctl start unit
```

立即停止某个单元：
```
systemctl stop unit
```

重新启动某个单元：
```
systemctl restart unit
```

让某个单元重新加载它的配置文件：
```
systemctl reload unit
```

允许某个单元在系统开机时运行：
```
systemctl enable unit
```

禁止某个单元在系统开机时运行：
```
systemctl disable unit
```

重置运行失败的单元状态：
```
systemctl reset-failed
```

读取某个单元的日志：
```
journalctl -u unit
```

统计日志占用的磁盘空间：
```
journalctl --disk-usage
```

### 电源管理

```
pacman -S polkit
```

重启：
```
systemctl reboot
```

关机：
```
systemctl poweroff
```

待机：
```
systemctl suspend
```

休眠：
```
systemctl hibernate
```

混合休眠模式（同时休眠到硬盘并待机）：
```
systemctl hybrid-sleep
```

### 单元文件
systemd单元文件可以从两个地方加载，优先级从低到高分别是：

/usr/lib/systemd/system/: 软件包安装的单元<br />
/etc/systemd/system/: 系统管理员安装的单元

定时器是以.timer为后缀的配置文件，记录由system的里面由时间触发的动作, 定时器可以替代cron的大部分功能。

修改单元文件之后重新加载
```
systemctl daemon-reload
```
