---
name: ghostty-config
description: Modify Ghostty terminal configuration files. Use whenever the user asks to change Ghostty settings, appearance, colors, fonts, keybindings, themes, window behavior, opacity/transparency, shell/command startup, cursor, clipboard, splits, tabs, quick terminal, macOS/Linux-specific options, or any other Ghostty behavior. Also use when the user points at Ghostty config files, asks where Ghostty config lives, asks how to reload Ghostty config, or asks to look up a Ghostty option from https://ghostty.org/docs.
---

# Ghostty Config Skill

Helps the user change Ghostty terminal appearance and behavior by editing its text config files. Ghostty's official docs are at `https://ghostty.org/docs`, with the configuration guide at `https://ghostty.org/docs/config` and the option reference at `https://ghostty.org/docs/config/reference`.

## Workflow

When the user asks for a Ghostty change, follow these steps in order:

1. **Understand the request.** Translate natural language into Ghostty concepts. "Make it transparent" usually maps to `background-opacity`. "Use JetBrains Mono" maps to `font-family`. "Change the theme" maps to `theme`. "Bind a key" maps to `keybind`.

2. **Find the active config files before editing.** Ghostty currently loads config files in this order, with later files overriding earlier files:
   - `$XDG_CONFIG_HOME/ghostty/config.ghostty`
   - `$XDG_CONFIG_HOME/ghostty/config`
   - If `XDG_CONFIG_HOME` is unset, use `$HOME/.config`.
   - On macOS, Ghostty also loads `$HOME/Library/Application Support/com.mitchellh.ghostty/config.ghostty` and `$HOME/Library/Application Support/com.mitchellh.ghostty/config` after the XDG files.
   Prefer editing an existing file that is already in use. If none exists, create `$HOME/.config/ghostty/config.ghostty` unless the user explicitly asks for the macOS Application Support path.

3. **Look up the option in official docs when needed.**
   - Start with `references/docs.md` for doc URLs, config format, reload notes, and local/offline lookup commands.
   - Use the official option reference at `https://ghostty.org/docs/config/reference` for exact key names, valid values, reload limitations, and platform caveats.
   - If Ghostty is installed, prefer local authoritative docs when available: `ghostty +show-config --default --docs` or installed docs under Ghostty's share/resources directory.
   - Do not rely on memory for obscure option names or values; Ghostty changes over time.

4. **Read the config file before editing.** Preserve the user's existing style: spacing around `=`, section comments, ordering, grouping, repeated keys, and any `config-file` includes.

5. **Make the smallest change that fulfills the request.**
   - Ghostty config syntax is `key = value`.
   - Keys are lowercase and case-sensitive.
   - Comments start with `#` on their own line.
   - Values may be quoted or unquoted.
   - Empty values reset a key to default.
   - Some keys may be repeated, such as `keybind` and `config-file`.
   If an option already exists, update it in place. If it is repeated, change only the relevant entry. If it does not exist, add it near related options.

6. **Validate when possible.** If Ghostty is on `PATH`, run a lightweight config command such as `ghostty +show-config` and inspect errors. If Ghostty is not installed or cannot run in the sandbox, visually check syntax and report that runtime validation was skipped.

7. **Tell the user how to reload.** Ghostty reloads by keybinding: `cmd+shift+,` on macOS and `ctrl+shift+,` on Linux by default. Some options require a full restart or only affect new windows/tabs; check the option docs and say so when relevant.

## Output Format

After editing, briefly tell the user:

- Which config file was changed
- What option(s) were set and to what values
- Why those keys match the request, citing the relevant official docs page if you looked it up
- Whether validation ran
- How to reload, including restart/new-window caveats when applicable

Do not paste the whole diff unless the user asks.

## Lookup-Only Questions

If the user asks "how do I do X in Ghostty" without asking for an edit, look up the official option and provide the minimal config snippet. Do not touch config files.

## Things That Look Like Ghostty Questions But Aren't

- Shell prompt, `$PS1`, Starship, zsh/fish/bash themes: this is shell configuration, not Ghostty.
- tmux panes, tmux prefix keys, tmux status line: tmux runs inside Ghostty; clarify whether the user means Ghostty or tmux before editing.
- Missing fonts: may be a font installation or font discovery issue. Check the requested family name and Ghostty font docs before changing unrelated options.
