# 配置Arch Linux

## 1、设定Locale
```
nano /etc/locale.gen
```

选定你需要的本地化类型(移除前面的＃即可), 比如中文系统可以使用:
> ```
  en_US.UTF-8 UTF-8
  zh_CN.GB18030 GB18030
  zh_CN.GBK GBK
  zh_CN.UTF-8 UTF-8
  zh_CN GB2312
  ```

然后运行：
```
locale-gen
```

创建locale.conf文件
```
echo LANG=en_US.UTF-8 > /etc/locale.conf
```

## 2、设置时区
```
echo Asia/Shanghai > /etc/timezone
```
或者使用`tzselect`命令来设置时区。

设置硬件时间标准为UTC，自动生成 /etc/adjtime
```
hwclock --systohc --utc
```

## 3、配置网络

设置机器名
```
echo computer_name > /etc/hostname
```

获取动态IP
```
systemctl start dhcpcd
systemctl enable dhcpcd
```

配置静态IP
> nano /etc/systemd/network/enp5s0.network
> ```
  [Match]
  Name=enp5s0
  [Network]
  Address=172.16.100.33/24
  Gateway=172.16.100.254
  DNS=202.101.172.35
  ```

子网掩码的对应关系
> ```
  /30 = 255.255.255.252
  /29 = 255.255.255.248
  /28 = 255.255.255.240
  /27 = 255.255.255.224
  /26 = 255.255.255.192
  /25 = 255.255.255.128
  /24 = 255.255.255.0
  /23 = 255.255.254.0
  /22 = 255.255.252.0
  /21 = 255.255.248.0
  /20 = 255.255.240.0
  /19 = 255.255.224.0
  /18 = 255.255.192.0
  /17 = 255.255.128.0
  /16 = 255.255.0.0
  ```

启动网卡
```
ip link set enp5s0 up
systemctl start systemd-networkd
systemctl enable systemd-networkd
```

设置域名解析
> nano /etc/resolv.conf
> ```
  # Google nameservers
  nameserver 8.8.8.8
  nameserver 8.8.4.4
  ```

NetworkManager and systemd-networkd are two different, mutually exclusive tools:
If use NetworkManager, stop and disable systemd-networkd.service and wpa_supplicant.service.
If use systemd-networkd, stop and disable NetworkManager.service and NetworkManager-dispatcher.service and enable and start systemd-resolved.service

自动连WiFi
```
pacman -S networkmanager network-manager-applet
systemctl start NetworkManager
systemctl enable NetworkManager
nm-applet 鼠标右键单击，编辑网络连接，设置WiFi密码，左键单击连接WiFi网络。
```

# 工作站{#workstation}

## 1、常用命令

sudo用于执行需要超级用户权限的命令
```
pacman -S sudo
EDITOR=nano visudo
```
> %wheel ALL=(ALL) NOPASSWD: ALL

执行大量pacman等需要超级用户权限的命令时，也可以用`su`切换到root帐号。

进程管理、域名信息、测量网站延迟
```
pacman -S htop whois httping
```

文件列表、查看、搜索
```
pacman -S exa bat fd ripgrep
```

## 2、命令解析器(shell)
```
pacman -S fish
```

## 3、图形界面

安装通用的显卡驱动(display driver)
```
pacman -S xf86-video-vesa mesa
```
可用`lspci | grep -e VGA -e 3D`命令识别显卡型号，再安装相对应的显卡驱动。



安装字体
```
pacman -S ttf-dejavu wqy-zenhei wqy-microhei
packer -S ttf-ms-fonts ttf-google-fonts-git
packer -S ttf-emojione-color
```

调节屏幕亮度
```
packer -S brightnessctl
brightnessctl -l
brightnessctl -c backlight set 50%
```

开启声音
```
pacman -S alsa-utils volumeicon
amixer sset Master unmute 解除各声道的静音
alsamixer
speaker-test -c 2 测试声卡是否工作
pacman -S pulseaudio pavucontrol 管理所有的声卡，对多个程序输出的声音混合处理
pulseaudio --start
pulseaudio --kill
```

```
aplay -l 获取声卡的声卡ID和对应的输出设备ID
aplay -D plughw:0,3 /usr/share/sounds/alsa/Front_Center.wav 测试HDMI输出声音（在该例中，0是声卡编号，3是设备编号）
nano ~/.asoundrc 修改配置文件把HDMI设置成默认设备
alsactl kill rescan 重新加载配置文件，然后重新打开播放声音的程序
```
```
pcm.!default {
  type hw
  card 0
  device 3
}
```

## 4、创建一个新的帐号，比如junfeng
```
useradd -m -G users,wheel,video,input -s /usr/bin/fish junfeng
passwd junfeng
```

重启电脑进入lightdm登录界面，用新建的帐号登录。
```
reboot
```

安装packer，用于从AUR安装软件
```
sudo pacman -S curl git expac jshon
mkdir packer && cd packer
curl https://aur.archlinux.org/cgit/aur.git/plain/PKGBUILD?h=packer -o PKGBUILD
makepkg
sudo pacman -U packer-*.pkg.tar.xz
cd ..
sudo rm -dR packer
```

安装输入法
```
pacman -S fcitx fcitx-im fcitx-ui-light fcitx-libpinyin fcitx-configtool
packer -S fcitx-sogoupinyin
fcitx -r 安装搜狗拼音后重启动fcitx
fcitx-config-gtk3
```
Ctrl + Space 激活输入法<br />
Ctrl + Shift 切换输入法<br />
Shift + Space 全角、半角切换<br />
左Shift 切换中英文<br />
-/= ↑↓ 向前/向后翻页

如果输入法不能激活，则`nano ~/.xprofile`，输入以下内容保存并重启电脑。
```
export GTK_IM_MODULE=fcitx
export QT_IM_MODULE=fcitx
export XMODIFIERS=@im=fcitx
```

安装GTK界面风格，我比较喜欢Arc Theme。
```
pacman -S arc-gtk-theme lxappearance-gtk3
```
运行lxappearance，然后选择Arc-Dark主题。

### Increasing the amount of inotify watchers
```
cat /proc/sys/fs/inotify/max_user_watches
echo fs.inotify.max_user_watches=524288 | sudo tee /etc/sysctl.d/40-max-user-watches.conf && sudo sysctl --system
```

## 5、常用软件

文件管理器
```
pacman -S mc pcmanfm-gtk3
```

压缩软件
```
pacman -S ark unrar p7zip
```

文本编辑器、剪贴板管理器、文本搜索
```
pacman -S medit clipit ripgrep
```

网页浏览器
```
pacman -S chromium
packer -S chromium-pepper-flash
chromium --proxy-server="socks5://127.0.0.1:1080"
```

图片浏览器
```
pacman -S gthumb
```

PDF阅读器
zathura快捷键: Tab显示目录，r旋转，q退出，/?搜索，as缩放，^d向下半页，^u向上半页，^n显隐状态栏
```
pacman -S evince zathura zathura-pdf-mupdf zathura-djvu zathura-ps
```
> nano ~/.config/zathura/zathurarc
> ```
set window-title-basename "true"
set selection-clipboard clipboard
set recolor true
set recolor-darkcolor "#93A1A1"
set recolor-lightcolor "#002B36"
```

音乐、视频播放器
```
pacman -S audacious smplayer mkvtoolnix
packer -S netease-cloud-music
```

办公软件
```
pacman -S libreoffice-fresh libreoffice-fresh-zh-CN
```

图像编辑、屏幕截图
```
pacman -S pinta scrot shutter
```

脑图创作
```
pacman -S xmind
```

电子邮件客户端、编程文档
```
packer -S n1 zeal
```

文件下载
```
pacman -S aria2 uget
```

FTP客户端
```
pacman -S filezilla
```

创建、编辑、刻录光盘文件
```
pacman -S brasero
```

挂载NTFS分区
```
pacman -S ntfs-3g
ntfs-3g /dev/sda1 /mnt/windows
```

挂载共享目录
```
pacman -S smbclient
mount -t cifs -o username=username,password="password" //16.187.190.50/test /mnt/
```

虚拟机
```
pacman -S virtualbox virtualbox-host-modules-arch virtualbox-guest-iso
```
创建一个虚拟机，安装Windows10系统，将某个本地目录配置成虚拟机中的共享目录。<br />
目前需要在Windows中运行的软件有QQ、迅雷、百度云同步盘。


# 服务器{#server}

## 创建虚拟内存
```
fallocate -l 2G /swapfile
chmod 600 /swapfile
mkswap /swapfile
swapon /swapfile
echo "/swapfile none swap defaults 0 0" >> /etc/fstab
```

## RAID磁盘阵列

先给每块硬盘创建一个分区（不用格式化），大于2T时使用GPT分区表，机械盘最好在末尾留出100M左右的未分配空间，从而确保每块硬盘分区的容量是一样的。
```
mdadm --create --verbose --level=5 --metadata=1.2 --chunk=256 --raid-devices=5 /dev/md0 /dev/sdb1 /dev/sdc1 /dev/sdd1 /dev/sde1 /dev/sdf1 --spare-devices=1 /dev/sdg1
cat /proc/mdstat
```
阵列构建完成后再格式化
```
mkfs.ext4 -v -L mdarray -m 0.01 -b 4096 -E stride=128,stripe-width=512 /dev/md0
mdadm --detail /dev/md0
mount /dev/md0 /data
```
最后更新/etc/fstab文件

扫描并恢复已有的磁盘阵列
```
mdadm --assemble --scan
mdadm --detail --scan --verbose > /etc/mdadm.conf
```

## SSH服务
```
pacman -S openssh
nano /etc/ssh/sshd_config
```
> ```
  PermitRootLogin yes
  UseDNS no
  ClientAliveInterval 60 #服务器端向客户端发送请求消息的时间间隔,以秒计
  ClientAliveCountMax 10 #允许客户端超时的次数
  TCPKeepAlive yes
  ```

```
systemctl start sshd
systemctl enable sshd
nano /etc/hosts.allow
```

加入客户端的公钥
```
nano ~/.ssh/authorized_keys
```

## FTP服务
```
pacman -S bftpd
nano /etc/bftpd.conf
```
> ALLOWCOMMAND_DELE="yes"

```
systemctl start bftpd
less /var/log/bftpd.log
bftpd start -d
```

## Samba服务

```
pacman -S samba
cp /etc/samba/smb.conf.default /etc/samba/smb.conf
nano /etc/samba/smb.conf
```

> ```
  [global]
  netbios name = archlinux-vm
  map to guest = Never
  usershare allow guests = no
  log file = /var/log/samba/%m.log
  max log size = 50000
  security = user
  encrypt passwords = yes
  smb passwd file = /etc/samba/smbpasswd
  interfaces = 10.0.2.4/24
  min protocol = SMB2
>
  [share]
  comment = share name
  path = /share
  browseable = yes
  writeable = yes
  inherit acls =yes
  create mask = 0770
  directory mask = 0770
  ```

```
systemctl start smbd
less /var/log/samba/smbd.log

useradd samba_user
userdel samba_user
smbpasswd -a samba_user

pdbedit -a -u samba_user
pdbedit -x -u samba_user
pdbedit --list --verbose
```

查看当前连接状态
```
smbstatus
```

windows 访问共享文件夹的帐号切换方法
1. 显示当前共享文件夹连接名
```
net use
```

2. 删除指定的连接名字
```
net use /del [连接名]
```
也可以通过net use /del * 选择删除

3. 指定登录帐号
```
net use [共享文件夹路径] /user:帐号名 密码
```
