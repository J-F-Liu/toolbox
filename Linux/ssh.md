# Secure Shell (SSH)

安装
```
pacman -S openssh
```

登录远程服务器
```
ssh username@server.address
```

免密码登录其他的SSH服务器
```
ssh-keygen
ssh-copy-id username@server.address
```

上传文件到远程主机
```
scp SourceFile user@host:directory/TargetFile
```

从远程主机下载文件
```
scp user@host:directory/SourceFile TargetFile
scp -r user@host:directory/SourceFolder TargetFolder
scp -P 2222 user@host:directory/SourceFile TargetFile
```

在远程主机上运行命令
```
ssh user@host 'ps ax | grep [h]ttpd'
```

本地端口转发（Local forwarding）
```
ssh -L <local-port-to-listen>:<target-host>:<target-port> <gateway>
```
通过gateway中转，连接本机的所监听的端口，相当于连上了目标机对应的端口。

远程端口转发
```
ssh -R <remote-port-to-listen>:<target-host>:<target-port> <remote-host>
```
经由远程机中转，访问远程机所监听的端口，相当于连上了目标机对应的端口，可以将目标机指定为本机。

端口转发需要在/etc/ssh/sshd_config中设置GatewayPorts yes。
