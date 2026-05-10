# Ghostty Docs Reference

Official docs:

- Main docs: `https://ghostty.org/docs`
- Configuration guide: `https://ghostty.org/docs/config`
- Full option reference: `https://ghostty.org/docs/config/reference`
- Keybindings: `https://ghostty.org/docs/config/keybind`
- Themes: `https://ghostty.org/docs/config/theme`

## Config Location

Ghostty config file names:

- `config.ghostty`
- `config` for older versions and compatibility

Load order:

1. `$XDG_CONFIG_HOME/ghostty/config.ghostty`
2. `$XDG_CONFIG_HOME/ghostty/config`
3. If `XDG_CONFIG_HOME` is unset, it defaults to `$HOME/.config`.
4. On macOS only, Ghostty also loads:
   - `$HOME/Library/Application Support/com.mitchellh.ghostty/config.ghostty`
   - `$HOME/Library/Application Support/com.mitchellh.ghostty/config`

Later files override earlier files when values conflict. Prefer editing an existing loaded config file. If no file exists, create `$HOME/.config/ghostty/config.ghostty` unless the user asks otherwise.

## Syntax

Ghostty uses a simple text format:

```conf
# Comments live on their own line.
key = value
font-family = JetBrains Mono
theme = Catppuccin Mocha
keybind = ctrl+d=new_split:right
```

Notes:

- Whitespace around `=` is optional.
- Keys are lowercase and case-sensitive.
- Values can be quoted or unquoted.
- Empty values reset the key to its default.
- Some keys are repeatable, including `keybind` and `config-file`.
- `config-file` may be repeated to split configuration into multiple files. Relative paths are relative to the file that contains the directive. Prefix optional includes with `?`.

## Local Documentation

When Ghostty is installed, these local sources are authoritative:

- `ghostty +show-config --default --docs`
- HTML/Markdown docs under Ghostty's installed `share/ghostty/docs` directory
- man pages under Ghostty's installed `share/man` directory

On macOS app installs, bundled resources live inside `Ghostty.app/Contents/Resources`.

## Reloading

Default reload keybindings:

- macOS: `cmd+shift+,`
- Linux: `ctrl+shift+,`

Some options cannot reload at runtime or only apply to new terminals. Confirm the caveat in the option reference for changes to GUI language, window creation behavior, platform-specific UI settings, and similar startup-level behavior.
