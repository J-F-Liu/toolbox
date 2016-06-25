## systemd

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
systemctl stopt unit
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

读取某个单元的日志：
```
journalctl -u unit
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

重新加载配置文件
```
systemctl daemon-reload
```
