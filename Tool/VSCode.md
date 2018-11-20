# Visual Studio Code

网站：https://code.visualstudio.com/

## 安装
```
packer -S visual-studio-code-bin
```

## 启动
```
code
```

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
