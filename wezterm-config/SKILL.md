---
name: wezterm-config
description: Modify wezterm terminal configuration in ~/.wezterm.lua. Use whenever the user asks to change wezterm settings, appearance, colors, fonts, key bindings, tab bar, transparency/opacity, window background, color scheme, cursor, scrollback, or any other wezterm behavior — even when they just say "change my terminal style", "make my terminal transparent", "bind a key in terminal", or point at ~/.wezterm.lua without naming wezterm. Also use when the user asks to look up a wezterm option or wonders how to configure something in wezterm.
---

# Wezterm Config Skill

Helps the user change wezterm terminal appearance and behavior by editing `~/.wezterm.lua`. The skill bundles a compact, searchable index of every documented wezterm config option so you don't need to re-fetch the official docs on every request.

## Workflow

When the user asks for a change, follow these steps in order:

1. **Understand the request.** Translate natural language into wezterm concepts. "Make it transparent" → `window_background_opacity`. "Dim the unfocused split" → `inactive_pane_hsb`. "Use JetBrains Mono" → `font` + `wezterm.font(...)`.

2. **Look up the option in the bundled references** (do NOT fetch the web unless the option is not in the references):
   - **Start with `references/all-options.md`** — it is the master index of every config field grouped by topic (Appearance, Fonts, Keys, Mouse, Launch/Domains, Window/Tabs, Scrolling, Cursor, Performance, Platform, etc.). Grep it first.
   - Then open the topic-specific reference for shape, defaults, and example values:
     - `references/appearance.md` — colors, color schemes, window background, tab bar, opacity, padding, inactive pane HSB
     - `references/fonts.md` — font, font_size, line_height, cell_width, font_rules, freetype/harfbuzz
     - `references/font-shaping.md` — ligatures, harfbuzz_features, custom block glyphs
     - `references/keys.md` — keys, leader, modifiers, key action names, key value formats
     - `references/key-tables.md` — modal key tables, ActivateKeyTable / PopKeyTable
     - `references/mouse.md` — mouse_bindings structure, focus-follow-mouse, hyperlink_rules
     - `references/launch.md` — default_prog, default_cwd, launch_menu, ssh/wsl/unix domains
     - `references/reload.md` — how the user applies config changes
   - Only fall back to fetching `https://wezterm.org/config/*.html` or `https://wezterm.org/config/lua/config/<option>.html` when the option isn't covered, or the user wants a detail (enum values, edge cases) that isn't in the local reference.

3. **Read `~/.wezterm.lua` before editing.** Preserve the user's existing style: tabs vs spaces, how they declare `config`, whether they use `wezterm.config_builder()`, their comment style, and the order of sections.

4. **Make the smallest change that fulfills the request.** Do not reformat unrelated code, do not "helpfully" add settings the user didn't ask for. If the option already exists in the file, update it in place. If it doesn't, add it near related options.

5. **Validate the Lua.** After editing, verify the file parses with `wezterm --config-file ~/.wezterm.lua ls-fonts --list-system 2>&1 | head -5` (any parse error surfaces on the first line) or just `luac -p ~/.wezterm.lua` if luac is available. If the user is on a machine without wezterm on PATH, skip this and just double-check the Lua syntax visually.

6. **Prompt the user about reloading.** End every edit with a short reload note. See `references/reload.md` — the key fact is wezterm watches the config file and auto-reloads on save by default, so usually nothing is needed. But the user should still know:
   - "Wezterm auto-reloads on save — your new windows and existing ones should pick it up. If you set `automatically_reload_config = false`, press `CTRL+SHIFT+R` inside wezterm to reload."
   - If the change affects only new panes/tabs (e.g. `default_prog`, `default_cwd`), say so explicitly.

## Output format

After editing, briefly tell the user:
- What option(s) you set and to what values
- The one-line "why" (which doc said this is the right field)
- Reload hint (one sentence, tailored to whether the change takes effect live or only in new panes)

Don't paste the whole diff back — the user can see it.

## When the user asks a lookup-only question

If the user asks "how do I do X in wezterm" without asking you to edit, just look up the answer in the references and give them the config snippet they'd paste. Don't touch `~/.wezterm.lua`.

## When the requested option isn't in the references

1. Try `https://wezterm.org/config/lua/config/<option_name>.html` — wezterm documents each config field at that URL pattern.
2. If that 404s, fetch the topic page from `references/all-options.md`'s category (e.g. appearance.html, keys.html).
3. After learning something new that the user is likely to ask about again, add it to the appropriate reference file so future invocations don't need the web.

## Things that look like wezterm questions but aren't

- Shell prompt / `$PS1` / starship → that's the shell, not wezterm. Tell the user and don't edit `~/.wezterm.lua`.
- tmux splits / panes → tmux lives inside wezterm; wezterm has its own pane system. Clarify which they mean before editing.
- Fonts "not showing up" → may be a font install issue, not a config issue. Check `font_dirs` and `wezterm ls-fonts` output.
