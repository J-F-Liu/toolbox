1. 用Arch Linux安装盘启动电脑，进入命令行界面。
2. 如果没有有线网络，先用iwctl命令连接WiFi网络：
```
> iwctl
station wlan0 get-networks
station wlan0 connect <SSID>
```
如果没有无线网卡的驱动，可用USB数据线连接手机，在手机打开【USB网络共享】。

3. 运行`archinstall`，进入安装界面。
4. 选择服务器镜像所在地区。
5. 磁盘分区，选择默认方案。
6. 登录认证，创建root账号和用户账户。
7. Profile Type选择Desktop/niri，polkit，其他用默认值
   不要选“niri - DankMaterialShell”，有两个问题：
   - dms-greeter是AUR软件包，会安装失败
   - DMS只安装软件包，不配置运行不起来
   等系统安装完成进入niri后，运行`curl -fsSL https://install.danklinux.com | sh`安装DMS。
8. Network Configuration 选择 Copy ISO network configuration to installation
9. Timezone 选 Asia/Shanghai
10. 执行Install，完成后重启电脑