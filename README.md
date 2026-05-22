# wsl-clipboard.yazi

让 Windows 和 WSL 里的 Yazi 共享一套文件剪贴板行为。

这个插件面向 Windows + WSL 用户：你可以在 Yazi 里按 `y` / `x` 复制或剪切文件，然后回到 Windows Explorer 里直接粘贴；也可以在 Windows Explorer 里复制或剪切文件，再回到 Yazi 里按 `p` 落盘。

## 功能

- `y`: 使用 Yazi 原生 yank，然后把文件列表同步到 Windows 剪贴板。
- `x`: 使用 Yazi 原生 cut，然后把文件列表同步到 Windows 剪贴板，并带上 move 语义。
- `p`: 从 Windows 剪贴板读取内容：
  - 文件/目录列表：复制或移动到当前 Yazi 目录。
  - 图片：弹出命名框，保存为图片文件。
  - HTML：保存为 `clipboard.html`。
  - 纯文本：忽略，不生成文件。
- `P`: 和 `p` 类似，但允许覆盖已有目标。
- `i` / `I`: 显式保存 Windows 剪贴板里的图片。
- `Y` / `X`: 清空 Yazi yank 状态，并清空 Windows 剪贴板。

## 适用范围

这是一个 Windows + WSL 桥接插件，不是通用 Linux/Wayland 剪贴板插件。

要求：

- Windows 10/11
- WSL
- Yazi 和 `ya`
- WSL 内能调用 `powershell.exe`
- WSL 内有 `timeout`、`wslpath`、`test`、`cp`、`mv`、`rm`、`realpath`

建议使用较新的 Yazi。插件开发和验证时基于 Yazi 26.x 的插件 API。

## 安装

如果你已经把这个仓库发布到 GitHub，并且仓库名是 `wsl-clipboard.yazi`：

```sh
ya pkg add <your-github-name>/wsl-clipboard
```

把 `<your-github-name>` 换成你的 GitHub 用户名。

注意：`ya pkg add` 只安装插件本体，不会替你修改快捷键配置。你仍然需要手动编辑 `~/.config/yazi/keymap.toml`。

然后把下面这段加入 `~/.config/yazi/keymap.toml`：

```toml
[mgr]
prepend_keymap = [
	{ on = "y", run = [ "yank", "plugin wsl-clipboard -- sync --copy" ], desc = "Yank to Windows clipboard" },
	{ on = "x", run = [ "yank --cut", "plugin wsl-clipboard -- sync --cut" ], desc = "Cut to Windows clipboard" },
	{ on = "p", run = "plugin wsl-clipboard -- paste", desc = "Paste from Windows clipboard" },
	{ on = "P", run = "plugin wsl-clipboard -- paste --force", desc = "Paste from Windows clipboard, overwrite" },
	{ on = "i", run = "plugin wsl-clipboard -- image", desc = "Save clipboard image" },
	{ on = "I", run = "plugin wsl-clipboard -- image --force", desc = "Save clipboard image, overwrite" },
	{ on = "Y", run = [ "unyank", "plugin wsl-clipboard -- clear" ], desc = "Clear Yazi yank and Windows clipboard" },
	{ on = "X", run = [ "unyank", "plugin wsl-clipboard -- clear" ], desc = "Clear Yazi yank and Windows clipboard" },
]
```

重启 Yazi 后生效。

## 本地开发安装

如果你还没有发布 GitHub，可以先手动安装：

```sh
mkdir -p ~/.config/yazi/plugins/wsl-clipboard.yazi
cp main.lua ~/.config/yazi/plugins/wsl-clipboard.yazi/main.lua
```

再把 `examples/keymap.toml` 里的内容合并进你的 `~/.config/yazi/keymap.toml`。

## 仓库结构

```text
wsl-clipboard.yazi/
├── main.lua
├── README.md
├── LICENSE
├── CHANGELOG.md
├── docs/
└── examples/
```

Yazi 包管理器安装时主要使用 `main.lua`、`README.md` 和 `LICENSE`。`docs/` 和 `examples/` 是给 GitHub 用户阅读的。

## 使用示例

### 从 Yazi 复制到 Windows

1. 在 Yazi 中选中文件或目录。
2. 按 `y`。
3. 回到 Windows Explorer。
4. 按 `Ctrl+V`。

### 从 Windows 复制到 Yazi

1. 在 Windows Explorer 中复制一个或多个文件。
2. 回到 Yazi，进入目标目录。
3. 按 `p`。

### 保存 Windows 截图

1. 用 `Win+Shift+S` 截图。
2. 回到 Yazi。
3. 按 `p`。
4. 输入文件名，或直接回车使用默认时间戳文件名。
5. 按 `Esc` 取消。

## 调试

如果需要看插件内部日志：

```sh
WSL_CLIPBOARD_DEBUG=1 YAZI_LOG=debug yazi
```

Yazi 日志通常在：

```sh
~/.local/state/yazi/yazi.log
```

## 边界

- 纯文本剪贴板在 Yazi 文件管理界面按 `p` 时会被忽略。
- 插件会短暂启动 PowerShell 探测 Windows 剪贴板，所以状态栏可能一闪 `1 left`。
- 这个插件不改变 rename/input 模式里的粘贴行为。
- 如果 WSL 无法访问 Windows 剪贴板、`powershell.exe` 不可用，插件无法工作。
- `P` / `I` 是覆盖语义，只应该在你确认目标目录安全时使用。
- `Y` / `X` 会清空 Windows 系统剪贴板，不只是清空 Yazi 的 yank 状态。
- 大文件或大目录复制可能运行较久，目前没有进度条。

## 文档

- [安装教程](docs/INSTALL.zh-CN.md)
- [测试清单](docs/TESTING.zh-CN.md)
- [故障排查](docs/TROUBLESHOOTING.zh-CN.md)
- [发布到 GitHub](docs/PUBLISH.zh-CN.md)

## License

BSD-2-Clause
