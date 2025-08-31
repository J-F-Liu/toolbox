# Windows 系统
winget install gitui
winget install lapce
winget install trippy
winget install zoxide

卸载 OneDrive

### 注册表设置

- 开机自动运行

```
HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Run
```

- 管理文件右键菜单

```
KEY_CLASSES_ROOT\*\shellex\ContextMenuHandlers
HKEY_CLASSES_ROOT\Directory\shellex\ContextMenuHandlers
```

- 禁止 Win10 自动安装游戏

```
HKEY_CURRENT_USER\SOFTWARE\Microsoft\Windows\CurrentVersion\ContentDeliveryManager
"FeatureManagementEnabled"=dword:00000000
"OemPreInstalledAppsEnabled"=dword:00000000
"PreInstalledAppsEnabled"=dword:00000000
"SilentInstalledAppsEnabled"=dword:00000000
```

```
HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\CloudContent

新建32 位 DWORD 值: DisableWindowsConsumerFeatures = 1
```

- 让 Windows 把 BIOS 硬件时间当作 UTC，修改后等 1 分钟生效

```
Reg add HKLM\SYSTEM\CurrentControlSet\Control\TimeZoneInformation /v RealTimeIsUniversal /t REG_DWORD /d 1
```

> 以管理员身份打开命令行程序，执行命令

### Win11 22H2 开启文件管理器多标签

用管理员身份运行 CMD 命令，然后进入 ViveTool 的目录，例如 CD C:\ViveTool-v0.3.1
依次输入命令：

vivetool /enable /id:37634385
vivetool /enable /id:39145991
vivetool /enable /id:36354489

重启系统就可以生效了。

### PowerShell

Set-ExecutionPolicy -Scope CurrentUser -ExecutionPolicy Unrestricted
Set-ExecutionPolicy RemoteSigned -Scope CurrentUser
Get-WindowsOptionalFeature -Online
列出环境变量 ls env:
设置环境变量 $env:VAR_NAME="value"
搜索命令历史：Ctrl+R

Path of PowerShell profile: "C:\Users\liujf\Documents\WindowsPowerShell\Microsoft.PowerShell_profile.ps1"

Set-Alias -Name "test" -Value "cargo test -- --nocapture --test-threads=1" -Option "AllScope"

function test([string]$name) {
    cargo test -- --nocapture --test-threads=1 $name
}

### [msys2](https://www.msys2.org/)

After install, add `C:\msys64\usr\bin` to system `Path` environment variable.

```
pacman -Syuu
pacman -S git make cmake
pacman -S fish

```

### Scoop

```
irm get.scoop.sh | iex

iwr https://gitee.com/scoop-bucket/scoop/raw/master/bin/install.ps1 -useb | iex
scoop config SCOOP_REPO https://gitee.com/scoop-bucket/scoop
scoop bucket add main https://gitee.com/scoop-bucket/main
scoop bucket add extras https://gitee.com/scoop-bucket/extras

scoop install starship
Invoke-Expression (&starship init powershell)

scoop install nodejs yarn python
scoop install llvm cmake
$env:LIBCLANG_PATH=C:\Users\Jefferey\scoop\apps\llvm\current\bin

scoop install lux
scoop install fd
scoop install helix
scoop install just
scoop install gitui
scoop install nnn
scoop install hyperfine

scoop bucket add extras
scoop install wezterm
scoop install nu
cargo-binstall zellij
scoop install file yazi
scoop install wasmer
wasmer run kherrick/pwgen -- -sy 20 1
```

### Rust

https://win.rustup.rs/x86_64

cargo install --list
cargo install procs
cargo install fd-find
cargo install --locked bat
cargo install --locked --features clipboard broot
cargo install kalker # 'Windows MSVC target is not supported (linking would fail)'
cargo install zellij
cargo install ripgrep
cargo install dufs
cargo install diskonaut
cargo install projectable
cargo install --git https://github.com/huss-a/rust-passgen.git
passgen -l 8

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

ssh -T git@gitee.com

git config --global user.email "china.liujunfeng@gmail.com"
git config --global user.name "Junfeng Liu"

### NeatDM
scoop install wget
wget https://www.neatdownloadmanager.com/file/NeatDM_setup.exe

### mpv

"C:\Users\cs\AppData\Roaming\mpv\input.conf"

### Cask

iwr https://cdn.jsdelivr.net/gh/cask-pkg/cask.rs/install.ps1 -useb | iex

- Email
- 便签

* 浏览器收藏夹
* 微信聊天记录
* Wechat Files
* Zotero
* MobaXterm.ini
* ~\AppData\Roaming\nushell\config.nu

### Check system file corruption

The sfc /scannow command (System File Check) scans the integrity of all protected operating system files and replaces incorrect, corrupted, changed, or damaged versions with the correct versions where possible.

1. Go to start>Type CMD, Right click and run as Administrator
2. Run `sfc /scannow` You may have to run this up to 3 times to fix all the problems until it reports "Windows did not find any integrity violations".

If you are on win 8 (and up) you should also run DISM whether SFC found errors or not!!

1. Run `DISM /Online /Cleanup-Image /RestoreHealth` to scan the image for component store corruption, perform repair operations automatically, and records that corruption to the log file. This generally takes 15-30 minutes depending on the corruption and size of the partition.
2. After running DISM it is a good idea to re-run `SFC /scannow` to make sure all the issues were fixed.

### WSL

WSL 2 需 BIOS/UEFI 中开启虚拟化（如 Intel VT-x/AMD - V）。

Run PowerShell or Windows Command Prompt as Administrator

```
❯ wsl --update
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

> wsl --install archlinux
> wsl -d archlinux
> wsl --shutdown
```

### ArchWSL

以管理员身份运行 PowerShell，BIOS 中已开启虚拟机功能。

1. 启用虚拟机功能
   > dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
2. 安装适用于 x64 计算机的 WSL2 Linux 内核更新包
   > https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi
3. 将 WSL 2 设置为默认版本
   > wsl --set-default-version 2
4. 启用 Windows 子系统，然后重启电脑
   > Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Windows-Subsystem-Linux
5. 安装 ArchWSL
   > scoop install archwsl
6. 打开 WIndows 终端，打开 Arch 标签页，更新 ArchLinux
   > pacman -Sy archlinux-keyring
   > pacman -Syu
7. 配置 wsl 启用 systemd:
   > echo -e "[boot]\nsystemd=true" | sudo tee -a /etc/wsl.conf
   > 通过 wsl --shutdown 命令关闭 wsl，来进行 wsl 的完整重启。查看是否启用了 Systemd
   > ps --no-headers -o comm 1

```
启用Windows子系统
  > dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
```

How does Windows decide whether your computer has limited or full Internet access?

Windows attempts to download a file from a dedicated Web server. Windows 10 or later versions: http://www.msftconnecttest.com/connecttest.txt.
If the download is successful and contains the correct contents, then Windows concludes that you have full Internet access.
If something goes wrong, Windows will report either limited or no Internet access, depending on what exactly went wrong.

创建服务
```
sc create MyService binPath= "C:\path\to\your\executable.exe -args" DisplayName= "My Service" obj= "NT AUTHORITY\LocalService" --ErrorControl=ignore
sc start MyService
sc delete MyService
```

https://nssm.cc/
```
nssm install <servicename>
nssm remove <servicename>
```


查看端口占用情况	netstat -ano | findstr "8080"
终止进程及其子进程 taskkill /F /T /PID 2668