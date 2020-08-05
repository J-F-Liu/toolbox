# WireGuard

Fast and secure kernelspace VPN

Wireguard is implemented as a kernel module, which is key to its performance and simplicity.

## 服务器配置

安装内核模块，版本与当前运行中的内核版本不一致时需要重启系统。

```
pacman -S wireguard-dkms
```

检查和加载 wireguard 内核模块

```
lsmod | grep wireguard
modprobe wireguard
```

生成秘钥

```
pacman -S wireguard-tools
cd /etc/wireguard
wg genkey | tee private | wg pubkey > public
chmod 600 private
```

启动 wireguard 虚拟网卡

```
cd /etc/wireguard
nano wg0.conf
wg-quick up wg0
```

wg0.conf 文件内容

```
[Interface]
PrivateKey = <本机的密钥>
Address = 10.10.10.1
ListenPort = 54321
PostUp = iptables -A FORWARD -i %i -j ACCEPT; iptables -A FORWARD -o %i -j ACCEPT; iptables -t nat -A POSTROUTING -o ens3 -j MASQUERADE
PostDown = iptables -D FORWARD -i %i -j ACCEPT; iptables -D FORWARD -o %i -j ACCEPT; iptables -t nat -D POSTROUTING -o ens3 -j MASQUERADE

[Peer]
PublicKey = <CLIENT PUBLIC KEY>
AllowedIPs = 10.10.10.2/32
```

AllowedIPs 属性对于出口流量来说是路由表，对于入口流量来说则是访问控制列表，参考 Cryptokey Routing。
PostUp 和 PostDown 配置用于在节点创建和删除之后执行的操作，对于 WireGuard 协议本身非必需。此处用于流量转发的 iptables 添加和删除，启动了代理服务器模式。

修改配置文件后，需重新启动虚拟网卡。

### 允许 IP 转发

Linux 内核可以将一个网卡上收到的数据包转发到另一个网卡上，转发规则由 iptables 定义。

If we are setting up a Linux router/gateway or maybe a VPN server or just a plain dial-in server then we will need to enable IP forwarding.

Check if IP Forwarding is enabled

```
sysctl net.ipv4.ip_forward
```

Enable IP Forwarding on the fly (without rebooting the system):

```
sysctl -w net.ipv4.ip_forward=1
```

Permanent setting using /etc/sysctl.conf:

```
net.ipv4.ip_forward = 1
```

## Linux 客户端

```
pacman -S wireguard-tools openresolv
```

```
[Interface]
PrivateKey = <本机的密钥>
Address = 10.10.10.2
DNS = 8.8.8.8

[Peer]
PublicKey = <server public key>
Endpoint = <server ip and udp port>
AllowedIPs = 0.0.0.0/0
```

## Mac 端配置

```
cargo install boringtun
sudo mkdir /etc/wireguard
wg genkey | tee private | wg pubkey > public
sudo nano /etc/wireguard/utun0.conf
sudo WG_QUICK_USERSPACE_IMPLEMENTATION=boringtun wg-quick up utun0
```

utun0.conf 文件内容

```
[Interface]
PrivateKey = <本机的密钥>
Address = 10.10.10.2
DNS = 8.8.8.8

[Peer]
PublicKey = <server public key>
Endpoint = <server ip and udp port>
AllowedIPs = 0.0.0.0/0
```

客户端上 AllowedIPs = 0.0.0.0/0，表示转发所有流量到 utun0，相当于全局代理，也可添加具体的允许 IP 网段。

## WireGuard 安卓客户端

https://play.google.com/store/apps/details?id=com.wireguard.android

## WireGuard Windows 客户端：https://tunsafe.com/download

教程：https://miguelmota.com/blog/getting-started-with-wireguard/
