# Changelog

All notable changes to this project will be documented in this file.

## v0.1.0

Initial public release.

### Added

- Sync Yazi file yank/cut operations to the Windows clipboard as `FileDropList`.
- Paste Windows Explorer file and folder clipboard contents into the current Yazi directory.
- Preserve copy vs move intent through Windows `Preferred DropEffect` when available.
- Save Windows clipboard images from Yazi with an interactive file-name prompt.
- Save HTML clipboard content as `clipboard.html`.
- Ignore plain text clipboard content in Yazi file-manager paste flow.
- Provide `p` unique-name behavior and `P` overwrite behavior.
- Provide `i` and `I` for explicit clipboard-image saving.
- Provide `Y` and `X` keymap examples that clear both Yazi yank state and the Windows clipboard.

### Notes

- This plugin targets Windows + WSL.
- The plugin has a single Yazi entry file: `main.lua`.
- Invoke it as `plugin wsl-clipboard -- <command>`.
- Do not invoke it as `plugin wsl-clipboard.<command>`.
- `ya pkg add` installs the plugin, but users still need to configure `keymap.toml` manually.
