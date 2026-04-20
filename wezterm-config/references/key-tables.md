# Key Tables (modal bindings)

Source: <https://wezterm.org/config/key-tables.html>.

Key tables enable tmux-style modal keymaps: press a prefix to enter a mode where keys mean different things, then leave the mode.

## Shape

```lua
local act = wezterm.action

config.leader = { key = 'Space', mods = 'CTRL|SHIFT' }

config.keys = {
  { key = 'r', mods = 'LEADER',
    action = act.ActivateKeyTable {
      name = 'resize_pane',
      one_shot = false,
    } },
}

config.key_tables = {
  resize_pane = {
    { key = 'LeftArrow',  action = act.AdjustPaneSize { 'Left',  1 } },
    { key = 'RightArrow', action = act.AdjustPaneSize { 'Right', 1 } },
    { key = 'UpArrow',    action = act.AdjustPaneSize { 'Up',    1 } },
    { key = 'DownArrow',  action = act.AdjustPaneSize { 'Down',  1 } },
    { key = 'Escape',     action = 'PopKeyTable' },
    { key = 'Enter',      action = 'PopKeyTable' },
  },
}
```

## ActivateKeyTable args

- `name` (string) — which table to push
- `one_shot` (bool, default true) — auto-pop after one matched key
- `timeout_milliseconds` (int) — auto-pop after this long of no key
- `replace_current` (bool) — swap top of stack instead of pushing
- `until_unknown` (bool) — pop when an unmapped key is pressed
- `prevent_fallback` (bool) — don't fall through to lower stack frames

## Stack actions
- `'PopKeyTable'` — pop top
- `'ClearKeyTableStack'` — clear all

## How matching works
Stack searched top→bottom. If top has no match, keeps looking unless `prevent_fallback = true`.

## Common modal use cases
- Resize panes (shown above)
- Vim-style copy mode (wezterm ships a `copy_mode` table; override with your own)
- Workspace switcher mode
- Window/tab move mode
