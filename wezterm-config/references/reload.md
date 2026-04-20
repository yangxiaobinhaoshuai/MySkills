# Reloading the config

Source: <https://wezterm.org/config/files.html>.

## Default behavior (auto-reload)

`automatically_reload_config = true` (default). Wezterm watches `~/.wezterm.lua` (or `$XDG_CONFIG_HOME/wezterm/wezterm.lua`) and **reloads on save automatically**. In practice, after editing you usually don't need to do anything — existing windows pick up the new values and new windows start fresh.

## If auto-reload is off

User set `config.automatically_reload_config = false`. To apply changes:
- Keyboard: `CTRL+SHIFT+R` inside any wezterm window.
- Command palette: `CTRL+SHIFT+P` → "Reload Configuration".

## When reload isn't enough (restart required)

A live reload re-evaluates the Lua file, but some options only take effect on new processes or new windows:

- `default_prog`, `default_cwd`, `set_environment_variables`, `launch_menu` entries — **new panes/tabs only**
- `ssh_domains`, `wsl_domains`, `unix_domains`, `exec_domains` — reconnect or reopen
- `front_end` (OpenGL vs WebGpu), `enable_wayland`, `dpi` — **fully restart wezterm**
- `initial_cols`, `initial_rows`, `enable_tab_bar` (Windows/macOS native title bar) — new windows only
- `window_decorations` — new windows only

## Debugging a broken config

If the config fails to parse, wezterm shows an error overlay instead of applying it. Check:
- `wezterm --config-file ~/.wezterm.lua show-keys` — runs the config once and reports errors
- `CTRL+SHIFT+L` — opens the debug overlay with log messages
- `wezterm.log_info(...)` inside your config prints to the debug overlay

## What to tell the user after an edit

Use one of these one-liners:

- Normal case: *"Auto-reload picks this up when you save. Existing windows refresh immediately."*
- New-pane only change: *"Applies to **new** panes/tabs — open a new one to see it. Existing panes keep their old `default_prog`/`cwd`/env."*
- Front-end/Wayland/DPI change: *"Fully quit and relaunch wezterm — this option is only read at startup."*
- If `automatically_reload_config = false` is in their config: *"Press CTRL+SHIFT+R to reload."*
