# Caddy

Install

```
sudo apt install -y debian-keyring debian-archive-keyring apt-transport-https
curl -1sLf 'https://dl.cloudsmith.io/public/caddy/stable/gpg.key' | sudo tee /etc/apt/trusted.gpg.d/caddy-stable.asc
curl -1sLf 'https://dl.cloudsmith.io/public/caddy/stable/debian.deb.txt' | sudo tee /etc/apt/sources.list.d/caddy-stable.list
sudo apt update
sudo apt install caddy
```

MacOS: brew install caddy
Docker: docker pull caddy

Usage

```
cat /usr/lib/systemd/system/caddy.service
sudo systemctl daemon-reload
sudo systemctl enable caddy
sudo systemctl start caddy
systemctl status caddy
journalctl -u caddy
sudo systemctl reload caddy
sudo systemctl stop caddy
cat /etc/caddy/Caddyfile
```

Serve static files

```
caddy file-server --root ~/mysite --listen :2015 --browse
```

Reverse proxy

```
caddy reverse-proxy --from :2016 --to 127.0.0.1:9000
```

## Caddyfile

```
Caddyfile = address directive+
          | [global_options_block] site_block+

global_options_block = '{'
    option+
'}'

site_block = address '{'
    directive+
'}'

directive = root
        | header
        | redir
        | rewrite
        | uri
        | try_files
        | basicauth
        | request_header
        | encode
        | templates
        | handle
        | handle_path
        | route
        | respond
        | metrics
        | reverse_proxy
        | php_fastcgi
        | file_server
        | acme_server
```

The order in which those directives are evaluated matters.
