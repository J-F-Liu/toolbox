# [fish](https://fishshell.com/)

## 安装

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

## 配置

fish starts by executing commands in ~/.config/fish/config.fish, you define your abbrs and aliases here.

```
nano ~/.config/fish/config.fish
fish_config 打开基于网页的配置界面
```

## 环境变量

Modify the $fish_user_paths universal variable, which is automatically prepended to $PATH.

```
> set PATH /usr/local/bin /usr/sbin $PATH
> set PATH[4] ~/bin
> echo $fish_user_paths
> echo $fish_user_paths[1]
> set fish_user_paths[4] /usr/local/Cellar/node/10.0.0/bin
> set -e $fish_user_paths[4] # Erase an arry element
> set -U fish_user_paths /home/junfeng/.local/bin /home/junfeng/.cargo/bin $fish_user_paths
> env PORT=8080 yarn start
> env (cat .env | xargs) program
```

## Universal Variables

Universal variables are shared across all instances of fish, now and in the future – even after a reboot.

```
> set -U EDITOR nano
```

## 常用命令

```
history search <keyword> 搜索以前执行过的命令
open <file_paht> 用默认的关联程序打开文件
source <script_file> 在当前Shell环境中执行脚本文件
type <command_name> 判断命令的类型，给出对应的函数定义或可执行文件的路径
exit 退出fish shell
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

## [Oh My Fish](https://github.com/oh-my-fish/oh-my-fish)

```
curl -L https://get.oh-my.fish | fish
omf install bobthefish
omf theme bobthefish
set -g theme_color_scheme solarized-dark
set -g theme_nerd_fonts yes
```
