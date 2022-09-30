卸载 OneDrive

### PowerShell

Set-ExecutionPolicy -Scope CurrentUser -ExecutionPolicy Unrestricted
Set-ExecutionPolicy RemoteSigned -Scope CurrentUser
Get-WindowsOptionalFeature -Online
列出环境变量 ls env:

### Rust

https://win.rustup.rs/x86_64

cargo install starship --locked
cargo install procs
cargo install fd-find
cargo install --locked bat
cargo install broot
cargo install kalker
cargo install zellij
cargo install ripgrep

### Git

ssh-keygen
cat .ssh/id_rsa.pub

nano ~/.ssh/config

```
# gitee
Host gitee.com
HostName gitee.com
PreferredAuthentications publickey
IdentityFile ~/.ssh/id_rsa
```

git config --global user.email "china.liujunfeng@gmail.com"
git config --global user.name "Junfeng Liu"

### Scoop

```
irm get.scoop.sh | iex

scoop install nodejs yarn python
scoop install llvm cmake
$env:LIBCLANG_PATH=C:\Users\Jefferey\scoop\apps\llvm\current\bin

scoop install lux
scoop install fd

scoop bucket add extras
scoop install wezterm
```

### Cask

iwr https://cdn.jsdelivr.net/gh/cask-pkg/cask.rs/install.ps1 -useb | iex

MobaXterm.ini
Wechat Files
Email
便签

### WSL

Run PowerShell or Windows Command Prompt as Administrator
```
❯ wsl.exe -l -o
以下是可安装的有效分发的列表。
使默认分发用 “*” 表示。
使用 'wsl --install -d <Distro>' 安装。

  NAME            FRIENDLY NAME
* Ubuntu          Ubuntu
  Debian          Debian GNU/Linux
  kali-linux      Kali Linux Rolling
  openSUSE-42     openSUSE Leap 42
  SLES-12         SUSE Linux Enterprise Server v12
  Ubuntu-16.04    Ubuntu 16.04 LTS
  Ubuntu-18.04    Ubuntu 18.04 LTS
  Ubuntu-20.04    Ubuntu 20.04 LTS

> wsl --install -d Ubuntu

> wsl --shutdown
```

### ArchWSL

以管理员身份运行 PowerShell，BIOS中已开启虚拟机功能。

1. 启用虚拟机功能
  > dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
2. 安装适用于 x64 计算机的 WSL2 Linux 内核更新包
  > https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi
3. 将 WSL 2 设置为默认版本
  > wsl --set-default-version 2
4. 启用Windows子系统，然后重启电脑
  > Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Windows-Subsystem-Linux
5. 安装ArchWSL
  > scoop install archwsl
6. 打开WIndows终端，打开Arch标签页，更新ArchLinux
  > pacman -Sy archlinux-keyring
  > pacman -Syu
```