#

### 开启 WSL2

在控制面板，找到程序选项，点击 “启用或关闭 Windows 功能”。
给 “适用于 Linux 的 Windows 子系统 ”打勾，确定。
在 Powershell 中，执行下面命令，切换成 WSL2 版本。

> wsl --set-default-version 2

### 安装 Ubuntu 子系统

从应用商店安装 Ubuntu 系统，这个系统将会以软件的形式存在。
在 Windows Terminal 右上角，有一个向下的箭头，点击它，就可以看到刚刚安装的 Ubuntu。

### 配置 Unbuntu

编辑 /etc/apt/sources.list 文件，把它的内容换成下面的源。

deb https://mirrors.ustc.edu.cn/ubuntu/ bionic main restricted universe multiverse
deb https://mirrors.ustc.edu.cn/ubuntu/ bionic-updates main restricted universe multiverse
deb https://mirrors.ustc.edu.cn/ubuntu/ bionic-backports main restricted universe multiverse
deb https://mirrors.ustc.edu.cn/ubuntu/ bionic-security main restricted universe multiverse
deb https://mirrors.ustc.edu.cn/ubuntu/ bionic-proposed main restricted universe multiverse
deb-src https://mirrors.ustc.edu.cn/ubuntu/ bionic main restricted universe multiverse
deb-src https://mirrors.ustc.edu.cn/ubuntu/ bionic-updates main restricted universe multiverse
deb-src https://mirrors.ustc.edu.cn/ubuntu/ bionic-backports main restricted universe multiverse
deb-src https://mirrors.ustc.edu.cn/ubuntu/ bionic-security main restricted universe multiverse
deb-src https://mirrors.ustc.edu.cn/ubuntu/ bionic-proposed main restricted universe multiverse

sudo apt install build-essential pkg-config
sudo apt install libssl-dev
cargo install --git https://github.com/skallwar/suckit
source: hyper::Error(Connect, ConnectError("tcp open error", Os { code: 2, kind: NotFound, message: "No such file or directory" }))

### 安装 Docker

安装 Docker，直接从官方下载最新的 Windows 版本就可以了。

docker 官方的镜像仓库无法访问。你可以从下面这些挑选一个，或者直接全部写上 。

["https://registry.docker-cn.com",
"https://dockerhub.azk8s.cn",
"https://reg-mirror.qiniu.com",
"http://hub-mirror.c.163.com",
"https://docker.mirrors.ustc.edu.cn"
]

apply & restart 重启生效

安装 Docker 的管理工具 portainer
