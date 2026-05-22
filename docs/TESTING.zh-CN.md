# 手动测试清单

## 基础检查

```sh
luac -p main.lua
taplo check examples/keymap.toml
```

## 纯文本 no-op

1. 在 Windows 中复制一段文本。
2. 在 Yazi 文件浏览界面按 `p`。

预期：

- 不生成文件。
- 不弹出输入框。
- 不长期停留在 `1 left`。

短暂 `1 left` 闪烁是正常现象，因为插件需要通过 PowerShell 探测 Windows 剪贴板类型。

## Windows 文件复制到 Yazi

1. 在 Windows Explorer 中复制一个文件。
2. 回到 Yazi，进入目标目录。
3. 按 `p`。

预期：文件出现在当前 Yazi 目录。

## Windows 多文件复制到 Yazi

1. 在 Windows Explorer 中复制多个文件。
2. 回到 Yazi，进入目标目录。
3. 按 `p`。

预期：多个文件都被复制到当前目录。

## Windows 目录复制到 Yazi

1. 在 Windows Explorer 中复制一个目录。
2. 回到 Yazi，进入目标目录。
3. 按 `p`。

预期：目录及其内容被复制到当前目录。

## Windows 剪切到 Yazi

1. 在 Windows Explorer 中剪切一个测试文件。
2. 回到 Yazi，进入目标目录。
3. 按 `p`。

预期：文件移动到当前目录。

## Yazi 复制到 Windows

1. 在 Yazi 中选中文件或目录。
2. 按 `y`。
3. 回到 Windows Explorer。
4. 按 `Ctrl+V`。

预期：Windows Explorer 可以粘贴对应文件或目录。

## Yazi 剪切到 Windows

1. 在 Yazi 中选中一个测试文件。
2. 按 `x`。
3. 回到 Windows Explorer。
4. 按 `Ctrl+V`。

预期：Windows Explorer 按移动语义处理。

## Windows 截图保存到 Yazi

1. 使用 `Win+Shift+S` 截图。
2. 回到 Yazi。
3. 按 `p`。
4. 输入文件名，或直接回车使用默认文件名。

预期：当前目录生成图片文件。

## 图片保存取消

1. 使用 `Win+Shift+S` 截图。
2. 回到 Yazi。
3. 按 `p`。
4. 输入框出现后按 `Esc`。

预期：不生成文件。

## 重名与覆盖

1. Windows 复制 `a.txt`。
2. Yazi 当前目录已有 `a.txt`。
3. 按 `p`。

预期：生成唯一文件名，例如 `a_1.txt`。

4. 再按 `P`。

预期：按覆盖语义处理目标文件。
