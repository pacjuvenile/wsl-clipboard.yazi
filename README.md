# wsl-clipboard.yazi

A Yazi plugin that bridges file and image clipboard operations between WSL and Windows.

`wsl-clipboard.yazi` makes Yazi inside WSL work with the Windows clipboard in a file-manager-friendly way:

- Yank files in Yazi and paste them in Windows Explorer.
- Copy files in Windows Explorer and paste them in Yazi.
- Save screenshots and other image clipboard data from Windows into the current Yazi directory.
- Ignore plain text in file-manager paste mode, so accidental text clipboard content does not create files.

## Requirements

- Windows 10 or Windows 11
- WSL
- Yazi and `ya`
- `powershell.exe` available from WSL
- `timeout`, `wslpath`, `test`, `cp`, `mv`, `rm`, and `realpath` available in WSL

This plugin is designed for Windows + WSL. It is not a general Linux, Wayland, X11, or macOS clipboard plugin.

## Installation

Install with Yazi's package manager:

```sh
ya pkg add pacjuvenile/wsl-clipboard
```

The GitHub repository is named `wsl-clipboard.yazi`, while the package name used by `ya pkg add` is `wsl-clipboard`.

`ya pkg add` installs the plugin only. Key bindings must be added manually to `~/.config/yazi/keymap.toml`:

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

Restart Yazi after changing the keymap.

## Manual Installation

For local testing or source installs:

```sh
mkdir -p ~/.config/yazi/plugins/wsl-clipboard.yazi
cp main.lua ~/.config/yazi/plugins/wsl-clipboard.yazi/main.lua
```

Then merge `examples/keymap.toml` into `~/.config/yazi/keymap.toml`.

## Usage

| Key | Action |
| --- | --- |
| `y` | Yank selected or hovered files in Yazi and sync them to the Windows clipboard as copy |
| `x` | Yank selected or hovered files in Yazi and sync them to the Windows clipboard as cut |
| `p` | Paste files, folders, HTML, or images from the Windows clipboard into the current Yazi directory |
| `P` | Paste from the Windows clipboard with overwrite behavior |
| `i` | Save image data from the Windows clipboard |
| `I` | Save image data from the Windows clipboard with overwrite behavior |
| `Y` / `X` | Clear Yazi yank state and the Windows clipboard |

## Clipboard Behavior

Clipboard formats are handled in this order:

1. Windows `FileDropList`
2. Image formats
3. HTML
4. Plain text

Plain text is intentionally ignored in `p` paste flow. Yazi is a file manager; this plugin does not materialize text clipboard content as `clipboard.txt`.

When a target already exists, `p` chooses a unique name such as `file_1.txt`. `P` uses overwrite behavior.

## Debugging

Run Yazi with debug logging:

```sh
WSL_CLIPBOARD_DEBUG=1 YAZI_LOG=debug yazi
```

Then inspect Yazi's log:

```sh
rg -n "wsl-clipboard|Clipboard" ~/.local/state/yazi/yazi.log
```

## Notes

- `P` and `I` can overwrite existing targets.
- `Y` and `X` clear the Windows system clipboard, not only Yazi's internal yank state.
- Large file or directory operations can run for a while and currently do not show progress.
- A short `1 left` status flash can appear while the plugin probes the Windows clipboard through PowerShell.
- Rename/input mode paste behavior is not changed by this plugin.

## More Documentation

- [Chinese installation guide](docs/INSTALL.zh-CN.md)
- [Chinese testing checklist](docs/TESTING.zh-CN.md)
- [Chinese troubleshooting guide](docs/TROUBLESHOOTING.zh-CN.md)

## License

BSD-2-Clause
