# Gnome

Run as root user
```
pacman -S gnome
systemctl enable gdm NetworkManager
reboot
```

Play H.264 video
```
pacman -S gst-plugins-ugly
```

Extensions
- All Windows
- Text Scaler
- Todo.txt
- Random Wallpaper

启用 fcitx
```
nano /etc/environment
```
> GTK_IM_MODULE=fcitx
> QT_IM_MODULE=fcitx
> XMODIFIERS=@im=fcitx
