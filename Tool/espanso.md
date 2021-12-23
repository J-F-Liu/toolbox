# Espanso - A Privacy-first, Cross-platform Text Expander

https://espanso.org/

```
espanso status
espanso path
espanso edit
espanso match list
espanso package list
espanso install basic-emojis
espanso restart
espanso help
```

- Toggle Key
  可以通过连续按两次 ALT(macOS 下为 Option) 按键来临时禁用 espanso。可以看到通知 Espanso disabled。 再按两次可以开启。
- Backspace Undo
  有些时候可能无意识中触发了 expansion，但又不想要这个结果，那么可以按一下 BACKSPACE 撤销这一次修改。

#### 自定义光标位置

在 espanso 的配置 replace 中可以使用 $|$ 作为占位符，表示光标。

比如想要输入 :div 的时候自动展开成为 <div></div> 然后将光标停留到中间，就可以使用：

```
- trigger: ":div"
  replace: "<div>$|$</div>"
```
