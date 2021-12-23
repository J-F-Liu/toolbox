## tmux

https://pragmaticpineapple.com/gentle-guide-to-get-started-with-tmux/

一个分屏工具，可用于远程连接，允许退出登录后仍保持会话继续运行。

创建且打开 tmux 的会话：

```
tmux new -s session-name
```

列出 tmux 的会话:

```
tmux ls
```

连接到上次的会话：

```
tmux a
```

连接某个会话：

```
tmux a -t session-name
```

从原始 Shell 把某个会话彻底关闭：

```
tmux kill-session -t session-name
```

让这个会话的当前窗口水平分割 : Ctrl + b --> %<br />
在分割的两个块中跳转 : Ctrl + b --> 方向键<br />
关闭某个分屏 : Ctrl + b --> x<br />
让这个会话断开连接，回到原始 Shell : Ctrl + b --> d
