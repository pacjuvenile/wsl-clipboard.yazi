# 测试清单

每次改插件后，建议按这个顺序测试。

## 1. 纯文本 no-op

1. 在 Windows 里复制一段文字。
2. 打开 Yazi，进入一个空目录。
3. 按 `p`。

预期：

- 不生成文件。
- 不弹框。
- 不长期卡住 `1 left`。

短暂闪一下 `1 left` 是正常的，因为插件需要调用 PowerShell 探测剪贴板类型。

## 2. Windows 文件复制到 Yazi

1. 在 Windows Explorer 里复制一个文件。
2. 回到 Yazi，进入目标目录。
3. 按 `p`。

预期：文件出现在当前 Yazi 目录。

## 3. Windows 多文件复制到 Yazi

1. 在 Windows Explorer 里复制多个文件。
2. 回到 Yazi，进入目标目录。
3. 按 `p`。

预期：多个文件都落盘。

## 4. Windows 目录复制到 Yazi

1. 在 Windows Explorer 里复制一个目录。
2. 回到 Yazi，进入目标目录。
3. 按 `p`。

预期：目录及其内容被复制过来。

## 5. Windows 剪切到 Yazi

1. 在 Windows Explorer 里剪切一个测试文件。
2. 回到 Yazi，进入目标目录。
3. 按 `p`。

预期：文件移动到当前目录。

## 6. Yazi 复制到 Windows

1. 在 Yazi 中选中文件。
2. 按 `y`。
3. 回到 Windows Explorer。
4. 按 `Ctrl+V`。

预期：Windows 能粘贴该文件。

## 7. Yazi 剪切到 Windows

1. 在 Yazi 中选中文件。
2. 按 `x`。
3. 回到 Windows Explorer。
4. 按 `Ctrl+V`。

预期：Windows 按移动语义处理。

## 8. Windows 截图保存到 Yazi

1. 用 `Win+Shift+S` 截图。
2. 回到 Yazi。
3. 按 `p`。
4. 看到命名框后直接回车。

预期：当前目录生成图片文件。

## 9. 图片取消

1. 用 `Win+Shift+S` 截图。
2. 回到 Yazi。
3. 按 `p`。
4. 命名框出现后按 `Esc`。

预期：不生成文件。

## 10. 覆盖行为

1. Windows 复制一个文件，例如 `a.txt`。
2. Yazi 目标目录已有 `a.txt`。
3. 按 `p`。

预期：生成唯一名称，例如 `a_1.txt`。

4. 再按 `P`。

预期：覆盖目标文件。
