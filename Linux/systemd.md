## systemd

Unit 是 Systemd 管理服务的基本单元，可以认为每个服务就是一个 Unit，并使用一个 Unit 文件定义。Unit 文件中需要包含相应服务的描述、属性以及需要运行的命令。

Target 是 Systemd 中用于指定服务启动组的方式。每次系统启动的时候都会运行与当前系统相同级别 Target 关联的所有服务，如果服务不需要跟随系统自动启动，则完全可以忽略这个 Target 的内容。

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
journalctl -u unit -r
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

systemd 单元文件可以从两个地方加载，优先级从低到高分别是：

/usr/lib/systemd/system/: 软件包安装的单元<br />
/etc/systemd/system/: 系统管理员安装的单元

定时器是以.timer 为后缀的配置文件，记录由 system 的里面由时间触发的动作, 定时器可以替代 cron 的大部分功能。

修改单元文件之后重新加载

```
systemctl daemon-reload
```

样例

```
[Unit]
Description=Nginx
After=syslog.target network.target

[Service]
Type=forking
Environment="VAR=VAL"
ExecStart=/opt/nginx/sbin/nginx
ExecReload=/opt/nginx/sbin/nginx -s reload

[Install]
WantedBy=multi-user.target
```

### 日志管理

```
  -r --reverse               Show the newest entries first
  -o --output=STRING         Change journal output mode (short, short-precise,
                               short-iso, short-iso-precise, short-full,
                               short-monotonic, short-unix, verbose, export,
                               json, json-pretty, json-sse, json-seq, cat,
                               with-unit)

```

查询日志占用的空间

```
journalctl --disk-usage
```

释放空间，指定保留多少

```
journalctl --vacuum-size=200M
```

配置最大占用量

```
nano /etc/systemd/journald.conf
> SystemMaxUse=500M
systemctl restart systemd-journald
```

### GUI

```
pacman -S systemd-ui
pacman -S gnome-logs
```
