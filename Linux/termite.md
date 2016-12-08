# Termite

网址：https://github.com/thestinger/termite

## 安装
```
pacman -S termite
```

## 快捷键
```
tab         auto-complete files and folder names
ctrl-shift-r	reload configuration file
ctrl-shift-c	copy to CLIPBOARD
ctrl-shift-v	paste from CLIPBOARD
ctrl-l  clear the Screen, similar to the clear command
ctrl-u  clear the line before the cursor position
ctrl-k  clear the line after the cursor
ctrl-y  paste the deleted command
ctrl-a,Home  go to the beginning of the line you are currently typing on
ctrl-e,End   go to the end of the line you are currently typing on
ctrl-c  kill whatever you are running or start a new prompt
ctrl-\  abort current program when terminal doesn't respond
shift-pageup	scroll up a page
shift-pagedown	scroll down a page
ctrl-shift-space	start selection mode
```

## 配置
```
mkdir ~/.config/termite
nano ~/.config/termite/config
```
```
[options]
scroll_on_output = false
scroll_on_keystroke = true
scrollback_lines = 10000
audible_bell = true
mouse_autohide = false
allow_bold = true
dynamic_title = true
urgent_on_bell = true
clickable_url = true
font = DejaVu Sans Mono 12
search_wrap = true

# "system", "on" or "off"
cursor_blink = system

# "block", "underline" or "ibeam"
cursor_shape = block

# Hide links that are no longer valid in url select overlay mode
filter_unmatched_urls = true

[colors]
foreground = #dcdccc
foreground_bold = #ffffff
background = #3f3f3f

# if unset, will reverse foreground and background
highlight = #2f2f2f

# colors from color0 to color254 can be set
color0 = #3f3f3f
color1 = #705050
color2 = #60b48a
color3 = #dfaf8f
color4 = #506070
color5 = #dc8cc3
color6 = #8cd0d3
color7 = #dcdccc
color8 = #709080
color9 = #dca3a3
color10 = #c3bf9f
color11 = #f0dfaf
color12 = #94bff3
color13 = #ec93d3
color14 = #93e0e3
color15 = #ffffff
```
