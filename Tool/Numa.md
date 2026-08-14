# [Numa](https://numa.rs/)

Every time you visit a website, you ask a DNS resolver where to go.
Numa is a DNS resolver that runs on your laptop or phone.

- Single binary, portable — macOS, Linux, and Windows
- Ad & tracker blocking — 385K+ domains, zero config
- TTL-aware caching (sub-ms lookups)
- Live dashboard with real-time stats and controls
- REST API — script everything, automate anything
- Name your local services with .numa domains
- LAN service discovery via mDNS

Named after Numa Pompilius — the second king of Rome who established the institutions, laws, and religious practices that outlasted the monarchy itself.

## Install

- Arch Linux: pacman -S numa
- Windows: download from GitHub Releases
- All platforms: cargo binstall numa

## Set as system DNS

- On macOS and Linux, numa runs as a system service (launchd/systemd). 
sudo numa install
sudo numa uninstall

- On Windows, numa auto-starts on login via registry.
numa install (admin) + reboot
numa uninstall (admin) + reboot

Windows also binds 127.0.0.2:53 (the built-in Dnscache owns 127.0.0.1:53).

Live dashboard: http://localhost:5380

## Other
numa help
numa service start
numa service stop
numa service status
numa service restart
numa block on|off
numa lan on|off

## Diagnose
```
> nslookup numa.numa 127.0.0.2
服务器:  UnKnown
Address:  127.0.0.2

非权威应答:
名称:    numa.numa
Addresses:  ::1
          127.0.0.1

> xh -v http://numa.numa
GET / HTTP/1.1
Accept: */*
Accept-Encoding: gzip, deflate, br, zstd
Connection: keep-alive
Host: numa.numa
User-Agent: xh/0.25.3

HTTP/1.1 200 OK
Cache-Control: no-cache
Content-Length: 59397
Content-Type: text/html; charset=utf-8
Date: Sat, 30 May 2026 06:21:51 GMT

<!DOCTYPE html>
......
```

如果出现：
```
>xh -v http://numa.numa
GET / HTTP/1.1
Accept: */*
Accept-Encoding: gzip, deflate, br, zstd
Connection: keep-alive
Host: numa.numa
User-Agent: xh/0.25.3

HTTP/1.1 502 Bad Gateway
Connection: close
Content-Length: 0
```
可能是设置了系统代理服务器，许多代理内核（Clash、Mihomo、Sing-box 等）有自己的 DNS 逻辑，不会使用系统配置。

YAML规则文件中加入：
```
dns:
  nameserver:
  - 127.0.0.2
```