# Wezterm All Config Options — Master Index

Grep this file first when looking up any wezterm config option. Source: <https://wezterm.org/config/lua/config/index.html>. For each option there is a page at `https://wezterm.org/config/lua/config/<option>.html` with full details.

## Appearance
- `background` — layered window background (images, colors, gradients with attachment/scale)
- `colors` — custom color palette (fg, bg, cursor, ansi, tab_bar, etc.)
- `color_scheme` — name of a built-in or custom scheme
- `color_schemes` — inline scheme definitions
- `color_scheme_dirs` — extra dirs to scan for scheme .toml files
- `bold_brightens_ansi_colors` — bold uses bright ANSI palette
- `foreground_text_hsb` — HSB transform applied to all text
- `force_reverse_video_cursor` — force reverse-video cursor
- `reverse_video_cursor_min_contrast` — contrast floor for reverse cursor
- `visual_bell` — flash screen on bell
- `audible_bell` — play sound on bell
- `window_background_gradient` — gradient background
- `window_background_image` — background image path
- `window_background_image_hsb` — HSB multipliers for bg image
- `window_background_opacity` — window transparency (0.0–1.0)
- `text_background_opacity` — per-cell bg opacity (0.0–1.0)
- `inactive_pane_hsb` — HSB transform for unfocused panes
- `window_padding` — {left, right, top, bottom} padding
- `window_decorations` — 'NONE' | 'TITLE' | 'RESIZE' | 'TITLE | RESIZE' | 'INTEGRATED_BUTTONS|RESIZE'
- `window_frame` — tab bar font/size/bg for fancy tab bar
- `window_content_alignment` — horizontal/vertical alignment of content
- `warn_about_missing_glyphs` — warn dialog on missing glyphs

## Fonts
- `font` — primary font (use `wezterm.font(...)`)
- `font_size` — point size
- `font_dirs` — extra font search dirs
- `font_locator` — system font resolver
- `font_rasterizer` — `FreeType` or CoreGraphics
- `font_shaper` — `Harfbuzz` or Allsorts
- `font_antialias` — antialiasing mode (deprecated; prefer freetype_render_target)
- `font_hinting` — hinting level (deprecated; prefer freetype_load_target)
- `font_rules` — per-style font overrides (bold/italic/intensity)
- `freetype_interpreter_version` — 35, 38, 40
- `freetype_load_flags` — FreeType load flags
- `freetype_load_target` — Normal / Light / Mono / HorizontalLcd / VerticalLcd
- `freetype_render_target` — Normal / Light / Mono / HorizontalLcd / VerticalLcd
- `freetype_pcf_long_family_names` — long family names for PCF
- `harfbuzz_features` — OpenType features e.g. `{'calt=0','clig=0','liga=0'}` to disable ligatures
- `cell_width` — multiplier on cell width
- `cell_widths` — per-char width overrides
- `line_height` — multiplier on line height
- `use_cap_height_to_scale_fallback_fonts` — scale fallbacks to primary cap height
- `anti_alias_custom_block_glyphs` — AA custom block glyphs
- `allow_square_glyphs_to_overflow_width` — allow wide glyphs
- `custom_block_glyphs` — wezterm draws its own block chars

## Keys & Input
- `keys` — list of key bindings
- `disable_default_key_bindings` — drop all defaults
- `leader` — modal leader `{key, mods, timeout_milliseconds}`
- `key_tables` — named modal key tables
- `key_map_preference` — `Mapped` (default) or `Physical`
- `use_ime` — enable IME
- `use_dead_keys` — enable dead keys
- `send_composed_key_when_left_alt_is_pressed`
- `send_composed_key_when_right_alt_is_pressed`
- `treat_left_ctrlalt_as_altgr` — Windows AltGr
- `enable_csi_u_key_encoding` — CSI-u protocol
- `enable_kitty_keyboard` — kitty keyboard protocol
- `debug_key_events` — log key events
- `swap_backspace_and_delete`
- `allow_win32_input_mode` — Win32 console input
- `bypass_mouse_reporting_modifiers` — default SHIFT

## Mouse
- `mouse_bindings` — list of `{event, mods, action, mouse_reporting?, alt_screen?}`
- `disable_default_mouse_bindings`
- `pane_focus_follows_mouse`
- `swallow_mouse_click_on_pane_focus`
- `swallow_mouse_click_on_window_focus`
- `mouse_wheel_scrolls_tabs`
- `hide_mouse_cursor_when_typing`
- `selection_word_boundary` — chars that delimit word selection
- `hyperlink_rules` — URL regex patterns

## Scrolling & Scrollback
- `scrollback_lines` — lines of history (default 3500)
- `scroll_to_bottom_on_input` — auto-scroll on input
- `alternate_buffer_wheel_scroll_speed` — wheel speed in altscreen
- `enable_scroll_bar` — show scrollbar
- `min_scroll_bar_height` — minimum thumb height

## Cursor & Text
- `default_cursor_style` — `SteadyBlock` / `BlinkingBlock` / `SteadyUnderline` / `BlinkingUnderline` / `SteadyBar` / `BlinkingBar`
- `cursor_thickness` — underline/bar width
- `cursor_blink_rate` — ms per blink half-cycle
- `cursor_blink_ease_in` / `cursor_blink_ease_out`
- `text_blink_rate` / `text_blink_rate_rapid`
- `text_blink_ease_in` / `text_blink_ease_out` / `text_blink_rapid_ease_in` / `text_blink_rapid_ease_out`
- `text_min_contrast_ratio` — min text contrast
- `underline_position` / `underline_thickness`
- `strikethrough_position`
- `normalize_output_to_unicode_nfc`

## Launch & Domains
- `default_prog` — shell to launch `{'/bin/zsh','-l'}`
- `default_cwd` — default working dir
- `default_domain` — which domain new panes use
- `default_workspace` — named workspace
- `default_mux_server_domain`
- `launch_menu` — launcher entries list of `SpawnCommand`
- `ssh_domains`
- `wsl_domains`
- `unix_domains`
- `tls_clients` / `tls_servers`
- `serial_ports`
- `ssh_backend` — `LibSsh` or `Ssh2`
- `default_ssh_auth_sock`
- `mux_enable_ssh_agent`
- `exec_domains`
- `set_environment_variables`

## Window & Tabs
- `enable_tab_bar`
- `hide_tab_bar_if_only_one_tab`
- `tab_bar_at_bottom`
- `tab_bar_style`
- `tab_max_width`
- `use_fancy_tab_bar`
- `show_tabs_in_tab_bar`
- `show_new_tab_button_in_tab_bar`
- `show_close_tab_button_in_tabs`
- `show_tab_index_in_tab_bar`
- `tab_and_split_indices_are_zero_based`
- `switch_to_last_active_tab_when_closing_tab`
- `prefer_to_spawn_tabs`
- `initial_cols` / `initial_rows`
- `quit_when_all_windows_are_closed`
- `window_close_confirmation`
- `exit_behavior` — `Close` / `Hold` / `CloseOnCleanExit`
- `exit_behavior_messaging`
- `integrated_title_buttons` / `integrated_title_button_style` / `integrated_title_button_alignment` / `integrated_title_button_color`
- `skip_close_confirmation_for_processes_named`
- `unzoom_on_switch_pane`
- `pane_select_font`
- `adjust_window_size_when_changing_font_size`
- `use_resize_increments`

## Performance & Rendering
- `max_fps` — frame cap
- `animation_fps` — animation FPS
- `status_update_interval` — ms
- `front_end` — `OpenGL` / `WebGpu` / `Software`
- `webgpu_power_preference` — `LowPower` / `HighPerformance`
- `webgpu_preferred_adapter`
- `webgpu_force_fallback_adapter`
- `prefer_egl` — Linux EGL vs GLX

## Platform-specific
- `enable_wayland` — Linux
- `tiling_desktop_environments` — names of tiling WMs
- `display_pixel_geometry` — subpixel layout
- `dpi` — override DPI
- `macos_window_background_blur` — blur radius
- `macos_fullscreen_extend_behind_notch`
- `native_macos_fullscreen_mode`
- `macos_forward_to_ime_modifier_mask`
- `win32_system_backdrop` — `Auto` / `None` / `Mica` / `Tabbed` / `Transient`
- `win32_acrylic_accent_color`
- `kde_window_background_blur`

## Character Sets & Hyperlinks
- `unicode_version` — e.g. `14`
- `treat_east_asian_ambiguous_width_as_wide`
- `hyperlink_rules` — URL/path detection regexes
- `quote_dropped_files` — quoting mode for drag-drop paths

## Selection & Quick Select
- `disable_default_quick_select_patterns`
- `quick_select_patterns`
- `quick_select_alphabet`
- `quick_select_remove_styling`
- `char_select_font` / `char_select_font_size` / `char_select_bg_color` / `char_select_fg_color`

## Command Palette
- `command_palette_font` / `command_palette_font_size`
- `command_palette_bg_color` / `command_palette_fg_color`
- `command_palette_rows`

## Notifications & IME
- `notification_handling` — `AlwaysShow` / `NeverShow` / `SuppressFromFocusedPane` / `SuppressFromFocusedTab` / `SuppressFromFocusedWindow`
- `detect_password_input`
- `ime_preedit_rendering` — `Builtin` / `System`

## Config Meta / Startup
- `automatically_reload_config` — default true; watch file and reload on save
- `check_for_updates` / `show_update_window`
- `clean_exit_codes`
- `launcher_alphabet`
- `canonicalize_pasted_newlines`
- `log_unknown_escape_sequences`
- `mux_env_remove`
- `ulimit_nofile` / `ulimit_nproc`
- `xim_im_name`
- `ui_key_cap_rendering`
- `daemon_options`
- `default_gui_startup_args`
