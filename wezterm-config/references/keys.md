# Keys

Source: <https://wezterm.org/config/keys.html>, <https://wezterm.org/config/default-keys.html>.

## Shape

```lua
local wezterm = require 'wezterm'
local act = wezterm.action

config.keys = {
  { key = 'm', mods = 'CMD', action = act.DisableDefaultAssignment },
  { key = 'Enter', mods = 'CMD', action = act.ToggleFullScreen },
  { key = '|', mods = 'CMD|SHIFT',
    action = act.SplitHorizontal { domain = 'CurrentPaneDomain' } },
}
```

## Modifiers

`CTRL`, `SHIFT`, `ALT` | `OPT` | `META`, `SUPER` | `CMD` | `WIN`, `LEADER`, `VoidSymbol`. Combine with `|`: `'CTRL|SHIFT'`.

## Key value formats

- **Named**: `Tab`, `Enter`, `Escape`, `Backspace`, `Space`, `F1`–`F24`, `Home`, `End`, `PageUp`, `PageDown`, `LeftArrow`, `RightArrow`, `UpArrow`, `DownArrow`, `Insert`, `Delete`, `NumLock`, `ScrollLock`
- **Character**: `'A'`, `'ñ'`
- **Physical position (ANSI US)**: `'phys:A'`
- **OS-mapped output**: `'mapped:a'`
- **Raw hardware code**: `'raw:123'`

## Leader (modal prefix)

```lua
config.leader = { key = 'a', mods = 'CTRL', timeout_milliseconds = 1000 }
-- then in keys use mods = 'LEADER'
```

## Common actions (under `wezterm.action`)

| Action | Purpose |
|---|---|
| `SendString { text = '...' }` | send literal text |
| `SendKey { key='a', mods='CTRL' }` | remap to another key |
| `Multiple { ... }` | chain multiple actions |
| `DisableDefaultAssignment` | unbind a default |
| `Nop` | no-op |
| `ActivateTab(n)` / `ActivateTabRelative(±n)` / `ActivateLastTab` | tab nav |
| `SpawnTab 'CurrentPaneDomain'` | new tab |
| `SpawnWindow` | new window |
| `CloseCurrentTab { confirm=true }` / `CloseCurrentPane { confirm=true }` | close |
| `SplitHorizontal { domain='CurrentPaneDomain' }` / `SplitVertical {...}` | split |
| `SplitPane { direction='Right', size={Percent=50} }` | split with size |
| `ActivatePaneDirection 'Left'/'Right'/'Up'/'Down'` | pane nav |
| `AdjustPaneSize { 'Left', 5 }` | resize pane |
| `TogglePaneZoomState` | zoom current pane |
| `RotatePanes 'Clockwise'/'CounterClockwise'` | rotate |
| `IncreaseFontSize` / `DecreaseFontSize` / `ResetFontSize` | font zoom |
| `CopyTo 'Clipboard'/'PrimarySelection'` / `PasteFrom ...` | clipboard |
| `ActivateCopyMode` / `QuickSelect` / `Search { ... }` | selection modes |
| `ShowLauncher` / `ShowLauncherArgs { flags='FUZZY|COMMANDS' }` | launcher |
| `ShowTabNavigator` / `ShowDebugOverlay` | overlays |
| `ActivateCommandPalette` | command palette |
| `ActivateKeyTable { name='...', one_shot=false, timeout_milliseconds=1000 }` | modal table (see key-tables.md) |

## Behavior flags

- `disable_default_key_bindings = true` — start from empty
- `key_map_preference = 'Physical'` — bare keys ignore keyboard layout
- `use_dead_keys = false` — dead keys emit immediately
- `use_ime = true` — route through OS IME
- `send_composed_key_when_left_alt_is_pressed = false`
- `send_composed_key_when_right_alt_is_pressed = true`
- `treat_left_ctrlalt_as_altgr = true` (Windows)
- `enable_csi_u_key_encoding = true` — modern modifier encoding
- `enable_kitty_keyboard = true` — kitty protocol
- `debug_key_events = true` — log to debug overlay

## Tips
- To discover default bindings: `wezterm show-keys --lua` prints them as Lua.
- If a key doesn't work, enable `debug_key_events` and watch `CTRL+SHIFT+L`.
