# 安装教程

这份文档按“小白步骤”写。

## 方案一：从 GitHub 安装

前提：插件仓库已经发布到 GitHub，仓库名是 `wsl-clipboard.yazi`。

1. 打开 WSL 终端。

2. 确认 `ya` 可用：

```sh
ya --version
```

如果提示找不到 `ya`，说明你的 Yazi 安装不完整，需要先处理 Yazi。

3. 安装插件：

```sh
ya pkg add <your-github-name>/wsl-clipboard
```

例子：

```sh
ya pkg add pacjuvenile/wsl-clipboard
```

注意：仓库名应是 `wsl-clipboard.yazi`，但 `ya pkg add` 里通常写 `wsl-clipboard`。

`ya pkg add` 不会自动改你的 `keymap.toml`。下一步必须手动配置快捷键。

4. 编辑 Yazi keymap：

```sh
nvim ~/.config/yazi/keymap.toml
```

把下面内容加入文件：

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

如果你的 `keymap.toml` 已经有 `[mgr]` 和 `prepend_keymap`，不要重复写第二个 `[mgr]`，只把数组里的几行合进去。

5. 完全退出 Yazi，再重新打开。

## 方案二：本地手动安装

适合你还没有把仓库发到 GitHub 的阶段。

1. 进入仓库目录：

```sh
cd /path/to/wsl-clipboard.yazi
```

2. 复制插件文件：

```sh
mkdir -p ~/.config/yazi/plugins/wsl-clipboard.yazi
cp main.lua ~/.config/yazi/plugins/wsl-clipboard.yazi/main.lua
```

3. 合并快捷键配置：

```sh
nvim ~/.config/yazi/keymap.toml
```

参考 `examples/keymap.toml`。

4. 重新打开 Yazi。

## 升级

如果你是通过 `ya pkg add` 安装的：

```sh
ya pkg upgrade
```

如果你是本地手动安装的，重新复制 `main.lua`：

```sh
cp main.lua ~/.config/yazi/plugins/wsl-clipboard.yazi/main.lua
```
