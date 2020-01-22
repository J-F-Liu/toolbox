Install package

```
apt install PACKAGE
apt remove PACKAGE
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
