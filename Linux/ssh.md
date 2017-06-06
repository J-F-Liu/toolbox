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
