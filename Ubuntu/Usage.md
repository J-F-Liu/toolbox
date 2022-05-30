Update System

```
apt update 下载最新的软件列表
apt upgrade 进行更新操作
apt autoremove 清理不需要的旧组件
do-release-upgrade -m server 升级大版本
```

Install package

```
apt show PACKAGE
apt install PACKAGE
apt remove PACKAGE
apt search PACKAGE
```

Search which package contains the file

```
apt install apt-file
apt-file update
apt-file search FILENAME
apt-file list PACKAGE
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

Install nodejs

```
apt install nodejs npm yarnpkg
```

Install Ruby

```
apt install ruby-dev
gem install bundle
apt install make g++ zlib1g zlib1g-dev
apt install libfreeimage3
cp /usr/lib/x86_64-linux-gnu/libfreeimage.so.3 /usr/lib/x86_64-linux-gnu/libfreeimage.so
```

Install Rust

```
curl https://sh.rustup.rs -sSf | sh
```

Install MongoDB

```
apt install mongodb
systemctl status mongodb
```

Other

```
apt install wireguard-dkms wireguard-tools
apt-get install goaccess
```
