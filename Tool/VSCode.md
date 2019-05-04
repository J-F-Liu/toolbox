# Visual Studio Code

网站：https://code.visualstudio.com/

### 安装
```
packer -S visual-studio-code-bin
```

### 启动
```
code
```

### 快捷键
Key     |   Effect
--------|---------------
Ctrl+/  | Toggle comment
Ctrl+]/[|	Indent/Outdent selection
Alt+↑/↓ | Move your selection up or down
Shift+Alt+↑/↓ |	Copy your selection and move it up or down
Ctrl+Shift+T | Reopen closed tab
Ctrl+P | Open any file in your root folder
Ctrl+F | Find in current file
Ctrl+Shift+F | Find in all files
F11 | Toggle Full Screen
F5 | Start debugging
F9 | Toggle Breakpoint

## 插件
- Align
- AutoFileName
- Duplicate action
- EditorConfig
- Excel Viewer
- Git History (git log)
- hexdump for VSCode
- Import Cost
- Live HTML Previewer
- Markdown Shortcuts
- Markdown Preview Enchanced
- Native Debug
- One Dark Theme
- Prettier
- REST Client
- SVG Viewer
- C#, Ruby, TOML, Docker

## 设置
```
{
    "editor.tabSize": 2,
    "editor.insertSpaces": true,
    "editor.renderWhitespace": "boundary",
    "vsicons.dontShowNewVersionMessage": true,
    "editor.lineHeight": 24,
    "editor.minimap.enabled": true,
    "workbench.statusBar.visible": true,

    "search.exclude": {
      "**/node_modules": true,
      "**/bower_components": true,
      "target": true
    },
    "search.useIgnoreFilesByDefault": true,
    "vsicons.projectDetection.disableDetect": true,
    "workbench.colorTheme": "One Dark",
    "workbench.iconTheme": "vscode-icons"
}
```
