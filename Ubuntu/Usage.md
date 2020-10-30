Update System
```
apt update 下载最新的软件列表
apt upgrade 进行更新操作
apt autoremove 清理不需要的旧组件
```

Install package

```
apt install PACKAGE
apt remove PACKAGE
```

Search which package contains the file

```
apt-file search FILENAME
```

Add User

```
adduser --system --shell /sbin/nologin --home /var/www caddy
groupadd caddy
usermod -aG caddy caddy
chsh -s /bin/false caddy
```

Enable and start systemd service in one command

```
systemctl enable --now caddy.service
systemctl status caddy
```

Install Ruby

```
apt-get install ruby-dev
gem install bundle
apt-get install make g++ zlibc zlib1g zlib1g-dev
```
```
apt install wireguard-dkms wireguard-tools
```