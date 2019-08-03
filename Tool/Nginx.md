# Nginx 配置

```
pacman -S nginx
systemctl start nginx
systemctl enable nginx
nano /etc/nginx/nginx.conf
less /etc/nginx/nginx.conf
systemctl reload nginx
journalctl -u nginx
less /var/log/nginx/access.log
```

启用gzip压缩
```nginx
gzip  on;
gzip_vary on;
gzip_proxied any;
gzip_comp_level 6;
gzip_min_length 2048;
gzip_types text/plain text/css application/javascript application/json;
```

```nginx
# Default server block for undefined domains
server {
    listen 80;
    return 404;
}
```

### 启用HTTPS和HTTP/2
```
pacman -S certbot

# Obtain a cert using a built-in “standalone” webserver (you may need to temporarily stop your existing webserver, if any)
certbot certonly --standalone -d example.com -d www.example.com

# Obtain a cert using the "webroot" plugin, which can work with the webroot directory of any webserver software
certbot certonly --webroot -w /var/www/example -d example.com -d www.example.com

# Renew certificates automatically before they expire
certbot renew --dry-run # test automatic renewal for your certificates
```

Configure SSL in Nginx
```nginx
server {
    listen 443 ssl http2;
    listen [::]:443 ssl http2;

    ssl on;

    ssl_certificate /etc/letsencrypt/live/yourdomain.com/cert.pem;
    ssl_certificate_key /etc/letsencrypt/live/yourdomain.com/privkey.pem;

    ssl_stapling on;
    ssl_stapling_verify on;
    ssl_trusted_certificate /etc/letsencrypt/live/yourdomain.com/fullchain.pem;

    ssl_session_timeout 5m;

    server_name yourdomain.com www.yourdomain.com;

    location / {
      root /var/www/yourpath;
      index index.html;
    }
}
```


Redirecting All Traffic to SSL/TLS
```nginx
server {
    listen 80;
    listen [::]:80;
    server_name example.com;
    return 301 https://$server_name$request_uri;
}
```

Create a systemd job for automatic renewal
> nano /etc/systemd/system/certbot.service

```
[Unit]
Description=Let's Encrypt renewal

[Service]
Type=oneshot
ExecStart=/usr/bin/certbot renew --quiet --agree-tos
ExecStartPost=/bin/systemctl reload nginx.service
```

> nano /etc/systemd/system/certbot.timer

```
[Unit]
Description=Daily renewal of Let's Encrypt's certificates

[Timer]
OnCalendar=daily
RandomizedDelaySec=1day
Persistent=true

[Install]
WantedBy=timers.target
```
Enable and start certbot.timer

### 增加请求体大小
The default value for client_max_body_size directive is 1 MiB.
```nginx
server {
    ...
    client_max_body_size 10M;
}
```
### 设置访问日志路径
```nginx
server {
    ...
    access_log /var/log/nginx/site.access.log;
}
```

### 生成访问量统计报表
```
pacman -S visitors goaccess
visitors /var/log/nginx/site.access.log > report.html
goaccess access.log -c
goaccess /var/log/nginx/site.access.log -o /var/www/html/report.html --log-format=COMBINED --real-time-html
```

### 运行单页应用
```nginx
server {
    ...
    location / {
        root /var/web/your_project/dist/client;
        try_files $uri /index.html;
    }
}
```

### 设置反向代理
```nginx
location /api/ {
    rewrite ^/api/(.*) /$1 break;
    proxy_set_header Host api.web-tinker.com;
    proxy_http_version 1.1;
    proxy_buffering  off;
    proxy_pass http://127.0.0.1:1234;
}
```
- rewrite 的作用是修改 $uri，但要注意 rewrite 要有个重新匹配 location 的副作用。由于 proxy_pass 的处理阶段比 location 处理更晚，所以这里需要 break 掉，以防止 rewrite 进入下一次 location 匹配而丢失 proxy_pass。
- hostname 可以通过 porxy_set_header 指令强制设置 proxy 的 HTTP 请求中的 Host 字段来修改它。
- proxy_pass 默认使用的是 http 1.0，可以通过 proxy_http_version 指令让它使用 http 1.1。
- 出现net::ERR_CONTENT_LENGTH_MISMATCH错误时关掉proxy_buffering。


### 参考资料

- [全面了解 Nginx 到底能做什么](https://www.jianshu.com/p/8bf73d1a758c)
