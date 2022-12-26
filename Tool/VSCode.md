# Visual Studio Code

网站：https://code.visualstudio.com/

教程：https://pawelcislo.com/2021/11/14/my-vs-code-playground/

### 安装

```
yay -S visual-studio-code-bin
```

### 启动

```
code
```

### 使用

Replacing text using regex groups, the captured groups can be recalled as $1, $2 etc in the replacement field of the same dialog.

### 快捷键

| Key              | Effect                                     |
| ---------------- | ------------------------------------------ |
| Ctrl+/           | Toggle comment                             |
| Ctrl+]/[         | Indent/Outdent selection                   |
| Alt+↑/↓          | Move your selection up or down             |
| Shift+Alt+↑/↓    | Copy your selection and move it up or down |
| Ctrl+Shift+T     | Reopen closed tab                          |
| Ctrl+P           | Open any file in your root folder          |
| Ctrl+F           | Find in current file                       |
| Ctrl+Shift+F     | Find in all files                          |
| Ctrl + Backspace | Delete previous word                       |
| Ctrl+I           | Trigger IntelliSense                       |
| F11              | Toggle Full Screen                         |
| F5               | Start debugging                            |
| F9               | Toggle Breakpoint                          |

## 插件

- Align
- Color Highlight
- Date & Time
- Duplicate action
- EditorConfig
- Excel Viewer
- Git Graph
- Hex Editor
- Image Tile Viewer
- Markdown All in One
- Markdown Preview Enchanced
- Native Debug
- One Dark Theme
- One Dark Syntax Theme
- REST Client
- SVG Viewer
- C#, Ruby, Docker
- TabNine
- Unicode code point of current character
- change-case
- Docker

### Web Dev

- AutoFileName
- Import Cost
- Prettier
- Live HTML Previewer

### Rust

- rust-analyzer (ext install matklad.rust-analyzer)
- CodeLLDB (ext install vadimcn.vscode-lldb)
- Better Toml (ext install bungcip.better-toml)
- Crates (ext install serayuzgur.crates)

### Font

- [Cousine](https://fonts.google.com/specimen/Cousine)
- [Consolas](https://docs.microsoft.com/en-us/typography/font-list/consolas)

## 设置

```
{
    "editor.formatOnSave": true,
    "editor.fontLigatures": true,
    "editor.fontFamily": "'Fira Code','Droid Sans Mono', 'monospace', monospace, 'Droid Sans Fallback'",
    "files.eol": "\n",
    "workbench.colorTheme": "Atom One Dark",
    "window.titleBarStyle": "custom",
    "terminal.integrated.copyOnSelection": true
}
```
