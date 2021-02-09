## 安装Manjaro

硬盘分区

1. create GUID Partition Table (GPT)
2. create the first partition as EFI partition
    - size of a few hundred MB
    - select fat32 as the file system
    - choose /boot/efi as the mount point
    - select the boot and esp flags from the list
3. created a ext4 partition mounted under the / directory
4. optional mount the /home directory on another partition


更新镜像服务器列表
```
sudo pacman-mirrors -f 0
```

安装Rust
```
sudo pacman -S rustup
RUSTUP_DIST_SERVER=http://mirrors.ustc.edu.cn/rust-static rustup update nightly
```

安装paru
```
sudo pacman -S --needed base-devel
git clone https://aur.archlinux.org/paru.git
cd paru
makepkg -si
```

安装输入法
```
pacman -S fcitx fcitx-im fcitx-ui-light fcitx-libpinyin fcitx-configtool
paru fcitx-sogoupinyin
```

重置root账号的密码
```
sudo su
passwd
```

```
sudo pacman -S lsd bat fd
paru procs gitui
alias ls='lsd -hA --group-dirs first'
alias tree='lsd --tree'
alias cat="bat"
alias ps="procs"
```