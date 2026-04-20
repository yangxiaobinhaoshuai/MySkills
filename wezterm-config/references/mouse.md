# Mouse

Source: <https://wezterm.org/config/mouse.html>.

## Shape

```lua
config.mouse_bindings = {
  -- Ctrl+click opens URL under cursor
  {
    event = { Up = { streak = 1, button = 'Left' } },
    mods = 'CTRL',
    action = wezterm.action.OpenLinkAtMouseCursor,
  },
  -- Disable default click-to-follow
  {
    event = { Up = { streak = 1, button = 'Left' } },
    mods = 'NONE',
    action = wezterm.action.CompleteSelection 'PrimarySelection',
  },
}
```

## Event shape

```lua
event = {
  -- one of:
  Down = { streak = 1|2|3, button = 'Left'|'Right'|'Middle' },
  Up   = { streak = 1|2|3, button = 'Left'|'Right'|'Middle' },
  Drag = { streak = 1, button = 'Left'|'Right'|'Middle' },
  -- or wheel:
  Down = { streak = 1, button = { WheelUp = 1 } },
  Down = { streak = 1, button = { WheelDown = 1 } },
}
```

Extra per-binding fields:
- `mouse_reporting = true|false` — only match when terminal mouse reporting is on/off
- `alt_screen = true|false|'Any'` — match on alt screen state

## Behavior flags

- `disable_default_mouse_bindings = true` — clear defaults
- `bypass_mouse_reporting_modifiers = 'SHIFT'` — modifier that suppresses app mouse capture
- `swallow_mouse_click_on_pane_focus` (bool)
- `swallow_mouse_click_on_window_focus` (bool)
- `pane_focus_follows_mouse` (bool) — hover to focus
- `hide_mouse_cursor_when_typing` (bool)
- `mouse_wheel_scrolls_tabs` (bool)

## Selection

- `selection_word_boundary = ' \t\n{}[]()"\'`'` — chars that break a double-click word

## Hyperlinks

```lua
config.hyperlink_rules = wezterm.default_hyperlink_rules()
-- or customize:
table.insert(config.hyperlink_rules, {
  regex = '\\b(JIRA-\\d+)\\b',
  format = 'https://jira.company/browse/$1',
  highlight = 1,
})
```
