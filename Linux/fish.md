# [fish](https://fishshell.com/)

##  安装
```
pacman -S fish
cat /etc/shells
chsh -s /usr/bin/fish
# log out and back in again
```

## 快捷键
```
↑和↓   切换历史命令
tab    尝试补全命令、参数、路径，在多个建议之间切换
→      补全自动建议的命令
Alt →  逐词补全
```

## 设置环境变量
```
> set PATH /usr/local/bin /usr/sbin $PATH
> set -U fish_user_paths /usr/local/bin $fish_user_paths
> env PORT=8080 yarn start
```

## Universal Variables
```
> set -U EDITOR nano
```

## 执行多个命令
```
> cp file1.txt file1_bak.txt; and echo "Backup successful"; or echo "Backup failed"
```

## 变量替换
```
> echo My home directory is $HOME
My home directory is /home/tutorial
> echo "My current directory is $PWD"
My current directory is /home/tutorial
> echo 'My current directory is $PWD'
My current directory is $PWD
```

## 命令替换
```
> echo In (pwd), running (uname)
In /home/tutorial, running FreeBSD
> alias rust-musl-builder='docker run --rm -it -v (pwd):/home/rust/src ekidd/rust-musl-builder:nightly'
```
