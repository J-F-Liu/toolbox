### micro 编辑器

一、基础导航与编辑
通用操作
```
Ctrl+E：打开命令面板
Ctrl+G：打开帮助文档
Ctrl+Q：退出编辑器
Ctrl+Z / Ctrl+Y：撤销/重做操作
Ctrl+F：搜索文本，Ctrl+N/Ctrl+P 切换搜索结果
```

光标移动
```
Ctrl+Left/Right：按单词左右跳转
Home/End：跳至行首/行尾
Ctrl+Home/Ctrl+End：跳至文档首/尾
```

二、文件与窗口管理
文件操作
```
Ctrl+S：保存文件
Ctrl+O：打开文件
Ctrl+Shift+S：另存为
```
分屏与标签页
```
Ctrl+\：垂直分屏（右分栏）
Ctrl+]：垂直分屏（左分栏）
Ctrl+T：新建标签页
Ctrl+Alt+>/Ctrl+Alt+<：切换下一/上一标签页
```
三、文本编辑与多光标

文本操作
```
Ctrl+K：剪切当前行
Ctrl+D：复制当前行
Ctrl+V：粘贴剪贴板内容
Alt+Backspace：删除左侧单词
```
多光标模式
```
Alt+Click：添加多光标
Alt+Shift+Up/Down：向上/下添加多光标
Alt+N：为匹配的单词添加多光标
Alt+M：在选中行首添加多光标
```
四、高级功能与自定义

插件与命令
```
Alt+B：运行当前文件（如 go run、python3）
Alt+F：格式化代码（如 black、go fmt）
Ctrl+E → plugin list：列出可用插件
Ctrl+E → set colorscheme solarized：切换主题
```

自定义键位
通过编辑 ~/.config/micro/bindings.json 自定义快捷键
示例：将 Ctrl+D 映射为删除行