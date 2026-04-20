# Fonts

Source: <https://wezterm.org/config/fonts.html>. Font family, size, fallback, freetype, harfbuzz.

## Primary font

```lua
config.font = wezterm.font 'JetBrains Mono'
config.font = wezterm.font('Fira Code', { weight = 'Bold', italic = true })
config.font_size = 13.0
```

## Fallback (glyph resolution order)

```lua
config.font = wezterm.font_with_fallback {
  'JetBrains Mono',
  'Noto Sans CJK SC',  -- CJK
  'Noto Color Emoji',  -- emoji
}
```

## Metrics

- `line_height` (ratio, 1.0 = font default) — e.g. `1.2` for airy, `0.95` for dense
- `cell_width` (ratio) — horizontal stretch
- `dpi` — override detected DPI (X11 HiDPI fix)

## Per-style overrides (`font_rules`)

```lua
config.font_rules = {
  { intensity='Bold', italic=true,
    font = wezterm.font('JetBrains Mono', { weight='Bold', italic=true, style='Italic' }) },
  { intensity='Half',
    font = wezterm.font('JetBrains Mono', { weight='Light' }) },
}
```
Match by `intensity = 'Normal'|'Bold'|'Half'`, `italic = true|false`.

## Ligatures / OpenType features

- `harfbuzz_features = { 'calt=0', 'clig=0', 'liga=0' }` — disable ligatures
- `harfbuzz_features = { 'zero', 'ss02' }` — enable stylistic sets (e.g. Fira Code slashed zero)

## Rendering (advanced)

- `freetype_load_target = 'Light'` — lighter hinting, often better on HiDPI
- `freetype_render_target = 'HorizontalLcd'` — subpixel AA
- `freetype_interpreter_version = 40` — modern interpreter
- `font_rasterizer = 'FreeType'` (default) — or CoreGraphics on macOS
- `font_shaper = 'Harfbuzz'` (default) — or `Allsorts`

## Discovery

- `font_dirs = { '/path/to/custom/fonts' }` — add to search path
- `warn_about_missing_glyphs = false` — quiet the popup
- `use_cap_height_to_scale_fallback_fonts = true` — normalize fallback sizes

## Useful commands

- `wezterm ls-fonts` — show resolved fonts for current config
- `wezterm ls-fonts --list-system` — every system font wezterm sees

## Top requests
1. Change font → `config.font = wezterm.font 'Fira Code'`
2. Add CJK support → `font_with_fallback` with Noto Sans CJK
3. Kill ligatures → `harfbuzz_features = {'calt=0','clig=0','liga=0'}`
4. Bigger text → bump `font_size`
5. Tighter lines → `line_height = 0.95`
