# 发布到 GitHub

下面按第一次开源发布来写。

## 1. 准备 GitHub 仓库

1. 打开 GitHub。
2. 点右上角 `+`。
3. 选择 `New repository`。
4. Repository name 填：

```text
wsl-clipboard.yazi
```

5. 选择 `Public`。
6. 不要勾选 `Add a README file`，因为本地已经有 README。
7. 点 `Create repository`。

这个仓库名很重要。Yazi 官方包管理器的单插件安装形式是：

```sh
ya pkg add owner/plugin-name
```

它对应的 GitHub 仓库通常是：

```text
owner/plugin-name.yazi
```

所以这里仓库必须叫 `wsl-clipboard.yazi`，安装命令才是 `ya pkg add <your-github-name>/wsl-clipboard`。

## 2. 初始化本地 git

进入仓库目录：

```sh
cd /workspace/wsl-clipboard.yazi
```

初始化：

```sh
git init
git add .
git commit -m "Initial release"
```

## 3. 连接 GitHub

把下面命令里的 `<your-github-name>` 换成你的 GitHub 用户名：

```sh
git branch -M main
git remote add origin https://github.com/<your-github-name>/wsl-clipboard.yazi.git
git push -u origin main
```

## 4. 自己安装验证

推上去以后，在 WSL 里执行：

```sh
ya pkg add <your-github-name>/wsl-clipboard
```

然后按 README 配好 `keymap.toml`。

## 5. 发第一个 tag

功能确认没问题后：

```sh
git tag v0.1.0
git push origin v0.1.0
```

## 6. GitHub Release

1. 打开 GitHub 仓库页面。
2. 点右侧 `Releases`。
3. 点 `Create a new release`。
4. Tag 选择 `v0.1.0`。
5. 标题写：

```text
v0.1.0
```

6. 描述可以写：

```text
Initial release.

- Sync Yazi y/x to Windows FileDrop clipboard.
- Paste Windows FileDrop files/directories into Yazi.
- Save Windows clipboard images from Yazi.
- Ignore plain text paste in file-manager mode.
```

7. 点 `Publish release`。

## 7. 后续更新流程

每次改完：

```sh
git status
git add .
git commit -m "Describe the change"
git push
```

如果要发新版本：

```sh
git tag v0.1.1
git push origin v0.1.1
```
