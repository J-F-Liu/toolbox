# Ubuntu Server

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

安装 Nginx

```
sudo apt install nginx
systemctl status nginx
sudo systemctl reload nginx
sudo systemctl restart nginx

sudo mkdir -p /var/www/example.com/
sudo chown -R $USER:$USER /var/www/example.com
sudo nano /etc/nginx/sites-available/example.com
sudo ln -s /etc/nginx/sites-available/example.com /etc/nginx/sites-enabled/
```

安装 Node.js

```
sudo apt install nodejs npm
nodejs -v
```

用 nvm 安装 Node.js

```
sudo apt purge nodejs
curl -sL https://raw.githubusercontent.com/creationix/nvm/v0.33.11/install.sh -o install_nvm.sh
bash install_nvm.sh
source ~/.profile
nvm install 8.11.1
nvm use 8.11.1
node -v
```

Other

```
apt install wireguard-dkms wireguard-tools
apt-get install goaccess
```
