# Appearance

Source: <https://wezterm.org/config/appearance.html>. Colors, color schemes, window background, tab bar, opacity.

## Colors

Built-in schemes: use `config.color_scheme = 'Batman'` — 700+ schemes available. Full list at <https://wezterm.org/colorschemes/index.html>.

- `color_scheme` (string) — name of scheme
- `color_scheme_dirs` (table) — `{ '/path/to/schemes' }`
- `color_schemes` (table) — inline definitions, `{ ['My Theme'] = { background = '#...' } }`
- `colors` (table) — fully custom palette; overrides scheme

### Common `colors.*` fields
- `foreground`, `background`
- `cursor_bg`, `cursor_fg`, `cursor_border`
- `selection_fg`, `selection_bg`
- `scrollbar_thumb`
- `split` — pane divider color
- `compose_cursor` — IME/dead key cursor
- `ansi` — list of 8 normal ANSI colors (black..silver)
- `brights` — list of 8 bright ANSI colors (grey..white)
- `indexed` — `{ [136] = '#af8700' }` for 256-color slots
- `copy_mode_active_highlight_bg/fg`, `copy_mode_inactive_highlight_bg/fg`
- `quick_select_label_bg/fg`, `quick_select_match_bg/fg`
- `input_selector_label_bg/fg`
- `launcher_label_bg/fg`

### Color formats
CSS3 names (`'silver'`), `#RRGGBB`, `rgb(r,g,b)`, `rgba(r,g,b,a)`, `hsl(H S% L%)`, `hsla(...)`, `hwb(H W% B%)`, `hsv(H S% V%)`.

## Tab Bar

- `enable_tab_bar` (bool, default true)
- `use_fancy_tab_bar` (bool, default true) — native style vs retro
- `hide_tab_bar_if_only_one_tab` (bool)
- `tab_bar_at_bottom` (bool)
- `tab_max_width` (int, retro only)
- `window_frame.font` = `wezterm.font { family = 'Roboto', weight = 'Bold' }`
- `window_frame.font_size` (number)
- `window_frame.active_titlebar_bg` / `inactive_titlebar_bg`

### `colors.tab_bar` fields (retro)
- `background` — strip color
- `inactive_tab_edge` — divider color (fancy mode)
- `active_tab = { bg_color, fg_color, intensity = 'Half'|'Normal'|'Bold', underline = 'None'|'Single'|'Double', italic, strikethrough }`
- `inactive_tab = { bg_color, fg_color }`
- `inactive_tab_hover = { bg_color, fg_color, italic }`
- `new_tab = { bg_color, fg_color }`
- `new_tab_hover = { bg_color, fg_color, italic }`

## Window background / opacity

- `window_background_opacity` (0.0–1.0) — whole-window alpha
- `text_background_opacity` (0.0–1.0) — per-cell bg alpha (useful with bg image)
- `window_background_image` (string) — path
- `window_background_image_hsb = { hue=1.0, saturation=1.0, brightness=0.3 }` — dim image
- `window_background_gradient = { orientation='Vertical', colors={'#000','#fff'} }`
- `background` (table) — advanced layered backgrounds with scale/tile

## Pane dimming / padding

- `inactive_pane_hsb = { saturation=0.9, brightness=0.8 }` — visually dim unfocused panes
- `window_padding = { left=2, right=2, top=0, bottom=0 }`

## macOS blur

- `macos_window_background_blur` (int) — blur radius in px, pairs with low `window_background_opacity` for "frosted glass"

## Top requests
1. Transparent background → `window_background_opacity = 0.85`
2. macOS frosted glass → `window_background_opacity = 0.7` + `macos_window_background_blur = 20`
3. Pick a theme → `color_scheme = 'Tokyo Night'`
4. Dim inactive pane → `inactive_pane_hsb = { saturation=0.8, brightness=0.7 }`
5. Hide tab bar when one tab → `hide_tab_bar_if_only_one_tab = true`
6. Retro tab bar → `use_fancy_tab_bar = false`
7. More breathing room → `window_padding = { left=8, right=8, top=4, bottom=4 }`
