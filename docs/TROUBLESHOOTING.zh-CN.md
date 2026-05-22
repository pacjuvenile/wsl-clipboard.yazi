# 故障排查

## 按键没有任何反应

确认插件已安装到：

```sh
~/.config/yazi/plugins/wsl-clipboard.yazi/main.lua
```

确认 `keymap.toml` 里使用的是：

```toml
plugin wsl-clipboard -- paste
```

不是：

```toml
plugin wsl-clipboard.paste
```

## 一直显示 `1 left`

短暂出现 `1 left` 是正常的。长期不消失才是问题。

排查：

```sh
WSL_CLIPBOARD_DEBUG=1 YAZI_LOG=debug yazi
```

退出后看日志：

```sh
rg -n "wsl-clipboard|Clipboard" ~/.local/state/yazi/yazi.log
```

## Windows 复制文件后，Yazi 按 `p` 失败

先确认 WSL 能调用 PowerShell：

```sh
powershell.exe -NoProfile -NonInteractive -Command '$PSVersionTable.PSVersion.ToString()'
```

再确认 WSL 能转换 Windows 路径：

```sh
wslpath -u 'C:\Windows'
```

如果这些命令失败，插件也会失败。

## Yazi 按 `y` 后 Windows 不能粘贴

确认 WSL 能把 Linux 路径转换成 Windows 可见路径：

```sh
wslpath -w "$PWD"
```

输出应该类似：

```text
\\wsl.localhost\Ubuntu\home\you\...
```

或者：

```text
C:\...
```

## 图片没有弹命名框

确认 Windows 剪贴板里确实是图片，而不是纯文本或文件。

用 PowerShell 检查：

```sh
powershell.exe -NoProfile -NonInteractive -STA -Command '
Add-Type -AssemblyName System.Windows.Forms
$d = [System.Windows.Forms.Clipboard]::GetDataObject()
$d.GetFormats()
'
```

如果只看到 `UnicodeText`、`Text`，说明剪贴板里不是图片。

## 纯文本按 `p` 没反应

这是预期行为。

Yazi 是文件管理器，这个插件不会把纯文本剪贴板自动写成 `clipboard.txt`，避免误生成垃圾文件。

## `P` 或 `I` 覆盖了文件

这是设计行为。

`P` 是强制粘贴，`I` 是强制保存图片。它们会按覆盖语义处理已有目标。第一次测试时建议在临时空目录里试。

## `Y` / `X` 后 Windows 剪贴板也没了

这是设计行为。

推荐 keymap 里 `Y` / `X` 同时执行：

```toml
{ on = "Y", run = [ "unyank", "plugin wsl-clipboard -- clear" ] }
{ on = "X", run = [ "unyank", "plugin wsl-clipboard -- clear" ] }
```

其中 `unyank` 清 Yazi 内部 yank 状态，`plugin wsl-clipboard -- clear` 清 Windows 系统剪贴板。

## 需要提交 issue 时带什么信息

请贴这些内容：

```sh
yazi -V
ya --version
uname -a
echo "$WSL_DISTRO_NAME"
command -v powershell.exe
command -v wslpath
```

再贴调试日志里相关行：

```sh
WSL_CLIPBOARD_DEBUG=1 YAZI_LOG=debug yazi
rg -n "wsl-clipboard|Clipboard" ~/.local/state/yazi/yazi.log
```
