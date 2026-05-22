# 安装

## 依赖

- Windows 10/11
- WSL
- Yazi 和 `ya`
- WSL 内可调用 `powershell.exe`
- WSL 内存在 `timeout`、`wslpath`、`test`、`cp`、`mv`、`rm`、`realpath`

检查基础依赖：

```sh
command -v yazi ya powershell.exe timeout wslpath
```

## 使用 `ya pkg` 安装

```sh
ya pkg add pacjuvenile/wsl-clipboard
```

GitHub 仓库名是 `wsl-clipboard.yazi`，安装时的包名是 `wsl-clipboard`。

`ya pkg add` 只安装插件文件，不会修改 Yazi 快捷键配置。安装后需要手动编辑：

```sh
~/.config/yazi/keymap.toml
```

加入：

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

如果 `keymap.toml` 已经有 `[mgr]` 和 `prepend_keymap`，只合并数组条目，不要重复创建第二个 `[mgr]` 表。

修改后重新启动 Yazi。

## 从源码安装

```sh
mkdir -p ~/.config/yazi/plugins/wsl-clipboard.yazi
cp main.lua ~/.config/yazi/plugins/wsl-clipboard.yazi/main.lua
```

然后把 `examples/keymap.toml` 合并到 `~/.config/yazi/keymap.toml`。

## 升级

通过 `ya pkg` 安装时：

```sh
ya pkg upgrade
```

手动安装时，重新复制 `main.lua`：

```sh
cp main.lua ~/.config/yazi/plugins/wsl-clipboard.yazi/main.lua
```
