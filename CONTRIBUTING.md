# Contributing

Issues and pull requests are welcome.

Please include:

- Windows version
- WSL distribution
- `yazi -V`
- `ya --version`
- whether `powershell.exe` and `wslpath` are available in WSL
- the clipboard source: Windows Explorer, Snipping Tool, browser, another app
- debug logs when possible

Before opening a pull request, run:

```sh
luac -p main.lua
```

If you changed the keymap example, also validate `examples/keymap.toml` with a TOML formatter or linter.
