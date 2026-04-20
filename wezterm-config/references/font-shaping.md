# Font Shaping

Source: <https://wezterm.org/config/font-shaping.html>.

## Ligatures

Most programming fonts (Fira Code, JetBrains Mono, Cascadia Code) ship ligatures on by default. To turn them off:

```lua
config.harfbuzz_features = { 'calt=0', 'clig=0', 'liga=0' }
```

To turn specific ones on (Fira Code example):
```lua
config.harfbuzz_features = { 'zero', 'onum', 'ss02', 'ss03' }
```
Features are OpenType 4-letter tags. See font vendor docs for which tags they support.

## Custom block glyphs (wezterm's own box-drawing)

- `custom_block_glyphs = true` (default) — wezterm draws its own block chars instead of using the font's, so they tile cleanly at any size
- `anti_alias_custom_block_glyphs = true` (default) — smooth edges
- `allow_square_glyphs_to_overflow_width = false` (default) — prevents e.g. emoji from spilling into neighbor cell

## Rasterizer / shaper

- `font_shaper = 'Harfbuzz'` (default) or `'Allsorts'`
- `font_rasterizer = 'FreeType'` (default on all platforms)
- `font_antialias = 'Greyscale' | 'Subpixel' | 'None'` (deprecated — prefer `freetype_render_target`)
- `font_hinting = 'Full' | 'Vertical' | 'VerticalSubpixel' | 'None'` (deprecated — prefer `freetype_load_target`)

## Misc

- `bold_brightens_ansi_colors = true` (default) — bold uses bright ANSI
- `adjust_window_size_when_changing_font_size = true` (default) — window resizes with zoom

## Per-style font_rules (see fonts.md)
Use `font_rules` for bold/italic variants that differ in family or weight. Useful for italic Operator Mono + Roman Fira Code combos.
