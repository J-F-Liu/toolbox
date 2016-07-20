# zsh

## 安装zsh
```
pacman -S zsh
chsh -s $(which zsh)
echo $SHELL
```

## 安装oh-my-zsh
```
sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```

## 配置oh-my-zsh
```
nano ~/.zshrc
```
> ```
ZSH_THEME=avit
plugins=(git sudo systemd)
```

## 常用命令
```
alias 列出所有的别名
fc-list | grep Mono 列出可用Mono字体
zsh --version 查看zsh版本号
upgrade_oh_my_zsh 升级oh-my-zsh
```
