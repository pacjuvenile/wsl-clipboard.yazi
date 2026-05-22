# 故障排查

## 按键没有触发插件

确认插件文件存在：

```sh
~/.config/yazi/plugins/wsl-clipboard.yazi/main.lua
```

确认 `keymap.toml` 使用单入口插件命令：

```toml
plugin wsl-clipboard -- paste
```

不要使用旧式子插件写法：

```toml
plugin wsl-clipboard.paste
```

## 长时间显示 `1 left`

短暂出现 `1 left` 是正常现象。长期不消失表示插件任务没有结束。

启用调试日志：

```sh
WSL_CLIPBOARD_DEBUG=1 YAZI_LOG=debug yazi
```

退出 Yazi 后检查日志：

```sh
rg -n "wsl-clipboard|Clipboard" ~/.local/state/yazi/yazi.log
```

## Windows 文件无法粘贴到 Yazi

检查 PowerShell：

```sh
powershell.exe -NoProfile -NonInteractive -Command '$PSVersionTable.PSVersion.ToString()'
```

检查 Windows 路径转换：

```sh
wslpath -u 'C:\Windows'
```

如果这些命令失败，插件无法从 Windows 剪贴板读取和落盘文件。

## Yazi 复制后 Windows Explorer 无法粘贴

检查当前路径是否能转换为 Windows 可见路径：

```sh
wslpath -w "$PWD"
```

输出应类似：

```text
\\wsl.localhost\Ubuntu\home\user\...
```

或：

```text
C:\...
```

## 图片没有进入命名框

检查 Windows 剪贴板是否暴露图片格式：

```sh
powershell.exe -NoProfile -NonInteractive -STA -Command '
Add-Type -AssemblyName System.Windows.Forms
$d = [System.Windows.Forms.Clipboard]::GetDataObject()
$d.GetFormats()
'
```

如果只看到 `UnicodeText` 或 `Text`，剪贴板中不是图片数据。

## 纯文本按 `p` 没有动作

这是设计行为。

插件不会把纯文本剪贴板内容写成 `clipboard.txt`。文件管理器中的 `p` 只处理文件、目录、图片和 HTML。

## `P` 或 `I` 覆盖了文件

这是覆盖命令的设计行为。

- `P`: 从 Windows 剪贴板强制粘贴，允许覆盖目标。
- `I`: 强制保存图片，允许覆盖目标。

建议先在临时目录中验证覆盖行为。

## `Y` / `X` 清空了 Windows 剪贴板

这是设计行为。

推荐 keymap 中 `Y` / `X` 同时执行：

```toml
{ on = "Y", run = [ "unyank", "plugin wsl-clipboard -- clear" ] }
{ on = "X", run = [ "unyank", "plugin wsl-clipboard -- clear" ] }
```

`unyank` 清除 Yazi 内部 yank 状态，`plugin wsl-clipboard -- clear` 清除 Windows 系统剪贴板。

## 提交 issue 时的信息

建议包含：

```sh
yazi -V
ya --version
uname -a
echo "$WSL_DISTRO_NAME"
command -v powershell.exe
command -v wslpath
```

以及相关日志：

```sh
WSL_CLIPBOARD_DEBUG=1 YAZI_LOG=debug yazi
rg -n "wsl-clipboard|Clipboard" ~/.local/state/yazi/yazi.log
```
