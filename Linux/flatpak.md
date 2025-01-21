官网：https://flathub.org/

添加仓库：
flatpak remote-add --if-not-exists flathub https://dl.flathub.org/repo/flathub.flatpakrepo

更换仓库：
sudo flatpak remote-modify flathub --url=https://mirror.sjtu.edu.cn/flathub

查看已安装应用：
flatpak list

安装 Flatpak 应用：
flatpak install <ApplicationID>
flatpak install <path/to/your/file.flatpakref>

运行 Flatpak 应用：
flatpak run <ApplicationID>

名称                      应用程序 ID                       版本                       分支              来源         安装
Google Chrome         com.google.Chrome                  131.0.6778.204-1            stable           flathub      system
Nutstore              com.jianguoyun.Nutstore            6.3.2                       stable           flathub      system
RustDesk              com.rustdesk.RustDesk              1.3.6                       stable           flathub      system
WeChat                com.tencent.WeChat                 4.0.1.11                    stable           flathub      system
Zed                   dev.zed.Zed                        v0.167.1                    stable           flathub      system


snap install procs
snap install bottom