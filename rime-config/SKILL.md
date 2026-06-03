---
name: rime-config
description: Modify Rime input method (中州韻 / 鼠鬚管 Squirrel / 小狼毫 Weasel / iBus-Rime / fcitx5-Rime) configuration via .custom.yaml patch files. Use whenever the user asks to change Rime settings, schemas, fuzzy pinyin (模糊音), candidate page size, key bindings, punctuation, simplified/traditional output, reverse lookup, app-specific behavior (Xcode 默认英文 etc.), Squirrel/Weasel fonts and color schemes, or any other Rime behavior — even when they just say "我的 Rime / 鼠须管 / 小狼毫 怎么…", "改一下输入法", "加个模糊音", "默认输出简体", or point at files like ~/Library/Rime/*.custom.yaml. Also use when the user asks how to look up a Rime option or wonders how a default.yaml / schema.yaml field works.
---

# Rime Config Skill

Helps the user change Rime input method behavior by editing `.custom.yaml` patch files in their Rime user folder. The skill bundles the official Rime wiki (Configuration、CustomizationGuide、SpellingAlgebra、RimeWithSchemata、UserData/SharedData) plus a topic-indexed cheat-sheet, so you don't need to re-fetch docs on every request.

## Background you must internalize

Rime config is YAML and lives in two layers:

- **Shared folder** (read-only defaults shipped with the frontend; do **not** edit there — upgrades overwrite it).
- **User folder** (your patches; this is where every change goes).

Per-platform user folder paths and the deploy procedure are in `references/data-folders.md`. Read it whenever the user hasn't told you their platform or you don't know where their config lives.

The canonical way to customize is to create `<config>.custom.yaml` next to (or instead of editing) `<config>.yaml`, containing a top-level `patch:` map whose keys are slash-paths into the original config tree. **Never** edit default `*.yaml` / `*.schema.yaml` from the shared folder directly — patches survive upgrades, edits don't.

## Workflow

When the user asks for a Rime change, follow these steps in order:

1. **Understand the request.** Translate natural language into Rime concepts. Common mappings:
   - "改每页候选数" → `menu/page_size`
   - "加模糊音 / zh ch sh 不分" → `speller/algebra` with `pinyin:/zh_z_bufen` etc.
   - "默认输出简体" → `switches` 中 `simplification.reset: 1`
   - "Xcode 里默认英文" → `app_options/com.apple.Xcode/ascii_mode: true` 写进 `squirrel.custom.yaml`
   - "改字体 / 配色" → `style/font_face` `style/color_scheme` 写进 `weasel.custom.yaml` 或 `squirrel.custom.yaml`
   - "改方案选单 / 启用五笔" → `schema_list` in `default.custom.yaml`
   - "翻页键改成 [ ]" → `key_binder/bindings`
   - "/ 键直接出顿号" → `punctuator/half_shape` 与 `punctuator/full_shape`
   - "反查改粤拼" → `reverse_lookup/dictionary`

2. **Look up the option in the bundled references** (do NOT fetch the web unless something is genuinely missing):
   - **Start with `references/all-options.md`** — topic-indexed map from "what the user wants" → which file, which YAML path. Grep this first.
   - Then deepen as needed:
     - `references/customization-guide.md` — full official cookbook of patch examples (page size, punctuation, simplified output, switches, key bindings, fuzzy pinyin, reverse lookup, app_options, Squirrel/Weasel appearance, custom phrases). This mirrors the upstream `CustomizationGuide` and is the most useful single reference.
     - `references/configuration-syntax.md` — the compile directives: `__patch`, `__include`, the `/+` (merge) / `/=` (replace) / `@0` `@last` `@before N` `@after N` `@next` (list addressing) operators, optional-include `?` suffix, `__append` / `__merge`.
     - `references/data-folders.md` — user/shared folder paths per platform, what each file does, deploy procedure, log locations.
     - `references/spelling-algebra.md` — full `speller/algebra` operator reference (`xlit`, `xform`, `derive`, `abbrev`, `fuzz`, `erase`); needed for non-trivial fuzzy pinyin, double pinyin, reverse-lookup spellings, or any new schema's pronunciation algebra.
     - `references/schemas.md` — full input-schema reference (engine pipeline, processors / segmentors / translators / filters, all component config keys). Use when designing a new schema or when the user asks about a less-common `*.schema.yaml` field.
   - Only fall back to fetching from `https://github.com/rime/home/wiki/...` when the option truly isn't covered, or the user wants a detail that isn't in the local references (a brand-new feature, an edge case in a specific librime version).

3. **Locate the user folder before editing.** From `references/data-folders.md`, pick the path for the user's platform. If you don't know the platform, ask once, then:
   - Check whether `<config>.custom.yaml` already exists.
   - If yes, **read it first** and preserve the user's existing `patch:` style, indentation (Rime style guide says 2 spaces), comment placement, and quoting conventions.
   - If no, create it with a brief comment header naming what it customizes.

4. **Make the smallest change that fulfills the request.**
   - Add the new key(s) under the existing `patch:` map. **Never** create a second `patch:` — YAML maps cannot have duplicate keys. If the file already has `patch:`, append your keys inside it.
   - If the key already exists under `patch:`, update its value in place.
   - For list-typed nodes (like `switches` or `schema_list`), follow the customization-guide pattern: either replace the entire list (clearest) or use `/+` / `@N` operators when surgical.
   - Don't reformat unrelated lines or "helpfully" add settings the user didn't ask for.

5. **Validate the YAML.** After editing, check that:
   - The file parses as YAML (`python3 -c "import yaml,sys; yaml.safe_load(open(sys.argv[1]))" <file>` if Python is available, or `ruby -ryaml -e 'YAML.load_file(ARGV[0])' <file>`).
   - There is exactly one top-level `patch:` key.
   - String values that contain `/`, `+`, `=`, `:`, `#`, leading symbols, or unicode are quoted (prefer single quotes per Rime's style guide).
   - List-type nodes you intended as list are `- item` style, not flow-style with `{}`.

6. **Tell the user how to redeploy.** Every patch needs a redeploy to take effect. Use the table in `references/data-folders.md` keyed to their frontend:
   - Squirrel: 系统输入法菜单 → 「重新部署」
   - Weasel: 开始菜单 → 小狼毫 → 重新部署（或托盘右键）
   - iBus-Rime: `touch ~/.config/ibus/rime/ && ibus restart`
   - fcitx5-Rime: 托盘 → Rime → 重新部署（或 `fcitx5 -r`）
   - 通用: 状态栏 ⟲ 按钮

   Mention that deploy can take a few seconds to a minute; if no Chinese can be typed during that window, that's normal. If you suspect the change might break compilation, add: "如果部署后输入法异常，看 `<log path>`（见 data-folders.md）里行首为 `E` 的记录。"

## Output format

After editing, briefly tell the user:
- Which file was created/edited (full path).
- Which keys you set and to what values (one line each).
- A one-line "why" — which option in which reference said this was the right field.
- Redeploy hint tailored to their frontend.
- If the change is one that only affects certain situations (e.g. `app_options` only fires inside that specific app, schema-scoped patches only inside that schema), say so explicitly.

Don't paste the full file back — the user can see the diff.

## When the user asks a lookup-only question

If the user just asks "Rime 怎么配置 X" without asking you to edit, look it up in the references and reply with the minimal `patch:` snippet they would paste. Don't touch any files. State which `.custom.yaml` it belongs in.

## When the requested option isn't in the references

1. Try the official wiki: `https://github.com/rime/home/wiki/CustomizationGuide`, `https://github.com/rime/home/wiki/Configuration`, or `https://github.com/rime/home/wiki/RimeWithSchemata`.
2. For frontend-specific options not in the references, look at the actual shipped defaults in the user's shared folder: `default.yaml`, `squirrel.yaml`, `weasel.yaml`, `<schema>.schema.yaml`. The keys you find there are the keys you can `patch:`.
3. After learning something the user is likely to ask about again, add it to the appropriate reference file (usually `all-options.md`) so future invocations don't need the web.

## Things that look like Rime questions but aren't

- 词库 / 词典里的某个词不出来 → 多半要改用户词典或自定义 `.dict.yaml`，不一定能用 `patch:` 解决。先确认到底是配置问题还是词典问题。
- 输入法切不出来 / 候选不显示 → 先看日志（见 data-folders.md），可能是 `.custom.yaml` 语法错误导致整方案编译失败，并非业务设置问题。
- macOS 系统输入源切换快捷键 → 这是系统设置，不在 Rime 配置里。
- 平面键盘 / 联想词排序优化 → 通常是 `translator` 参数（如 `enable_user_dict`、`spelling_hints`）或词典字频问题，而不是「外观」类设置。

## 注意事项

- Rime 用户文件夹的 `installation.yaml` 和 `user.yaml` 是程序运行时写入的，**不要**手动改这两个。
- `~/Library/Rime/build/` 下是编译产物，调试时可以**查看**确认补靪是否生效，但**不要**直接改 —— 重新部署会覆盖。
- 升级 librime 后某些键可能改名或废弃，发现日志报 `unknown option` 时去查 `references/customization-guide.md` 最新示例或官方 wiki。
