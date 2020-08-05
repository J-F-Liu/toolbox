# Caddy

Serve static files

```
caddy file-server --root ~/mysite --listen :2015 --browse
```

Reverse proxy

```
caddy reverse-proxy --from :2016 --to 127.0.0.1:9000
```
