# i3 Window Manager

## 常用快捷键
$mod一般为Alt或Win键，初次启动i3时可进行设置。
* $mod + enter: opens a new terminal
* $mod + d: open application launcher (dmenu)
* $mod + shift + q: kill a window
* $mod + shift + e: exit i3
* $mod + shift + c: reload i3 configuration
* $mod + shift + r: restart i3
* $mod + shift + arrow keys: move window
* $mod + number: switch to another work space
* $mod + shift + number: move a window to another work space
* $mod + v: split a window vertically
* $mod + h: split a window horizontally
* $mod + e: toggle container layout between horizontally and vertically
* $mod + w: tabbed container layout
* $mod + w: stacking container layout
* $mod + f: toggle fullscreen
* $mod + shift + space: toggle floating
* $mod + mouse left button: drag floating
* $mod + r: enter resize mode
  * arrow keys: resize window
  * Esc: exit resize mode

## 配置文件的路径

* /etc/i3/config
* ~/.i3/config
* ~/.config/i3/config
* /etc/i3blocks.conf
* ~/.i3blocks.conf

```
# run `xprop` and click target window to get class name
assign [class="chromium"]  →  3: chromium
assign [class="Krusader"]  →  4: krusader
assign [class="google-chrome"]  →  5: chrome

# Dsplay a workspace bar
bar {
        status_command i3blocks
}

bindsym $mod+End exec /bin/bash -c 'i3lock -i <(import -window root - | convert -blur -5x5 - png:-)'
bindsym $mod+b border toggle
hide_edge_borders smart

exec --no-startup-id fcitx
exec --no-startup-id clipit
exec --no-startup-id volwheel
```

## 指定终端模拟器程序
```
# start a terminal
#bindsym $mod+Return exec i3-sensible-terminal
bindsym $mod+Return exec termite
```
