# Rime 配置主索引

**用法：** 先在这里 grep，找到「我要改的东西在哪个文件、哪个配置路径」，再去 `customization-guide.md` 看示例，或去用户/共享文件夹打开对应源文件确认默认值。

格式约定：
- **配置路径** 用 `a/b/c` 表示 YAML 节点的层级，对应 `patch:` 中的字符串 key。
- **目标文件**指应该建立哪一个 `.custom.yaml` 来写补靪。
- 标 *(全局)* 的设置写在 `default.custom.yaml`；标 *(方案)* 的写在 `<schema_id>.custom.yaml`；标 *(前端)* 的写在 `squirrel.custom.yaml` / `weasel.custom.yaml` / `ibus_rime.custom.yaml`。

---

## 一、方案选单与切换

| 想做的事 | 目标文件 | 配置路径 | 说明 |
| --- | --- | --- | --- |
| 启用 / 排序输入方案列表 | `default.custom.yaml` *(全局)* | `schema_list` | 列表型，整体替换。列表项格式 `- schema: <id>` |
| 修改方案选单快捷键 | `default.custom.yaml` *(全局)* | `switcher/hotkeys` | 默认 `Control+grave`、`F4`。列表型，整体替换 |
| 切换方案时按钮顺序 | `default.custom.yaml` | `switcher/save_options` | 列表，记忆哪些开关 |
| 关掉「逐键提示」（仅码表方案） | `<schema>.custom.yaml` *(方案)* | `translator/enable_completion` | 布尔，默认 `true` |

## 二、候选窗 / 编码行

| 想做的事 | 目标文件 | 配置路径 | 说明 |
| --- | --- | --- | --- |
| 每页候选数（1~9，部分前端到 10） | `default.custom.yaml` 或 `<schema>.custom.yaml` | `menu/page_size` | 整数 |
| 翻页 / 选字按键 | `<schema>.custom.yaml` 或 `default.custom.yaml` | `key_binder/bindings` | 列表，整体替换或追加 `/+` |
| 是否启用整句输入（仅码表） | `<schema>.custom.yaml` | `translator/enable_sentence` | 布尔 |
| 是否启用用户词典 / 字频调整 | `<schema>.custom.yaml` | `translator/enable_user_dict` | 布尔 |

## 三、按键 / 输入习惯

| 想做的事 | 目标文件 | 配置路径 | 说明 |
| --- | --- | --- | --- |
| 中英文切换键（Caps、Shift 等） | `default.custom.yaml` 或 `<schema>.custom.yaml` | `ascii_composer/switch_key` | map：`Caps_Lock: commit_code` 等。可选值 `inline_ascii` / `commit_text` / `commit_code` / `noop` |
| Caps Lock 行为 | `default.custom.yaml` | `ascii_composer/good_old_caps_lock` | 布尔。`true`=Caps 切换西文模式后输出大写字母 |
| 自定义按键绑定（翻页、选字、清空） | `<schema>.custom.yaml` | `key_binder/bindings` | 列表项形如 `{ when: has_menu, accept: bracketright, send: Page_Down }` |
| 识别为西文的输入串（白名单） | `default.custom.yaml` 或 `<schema>.custom.yaml` | `recognizer/patterns/<key>` | 正则，如 `"^rime[0-9]+$"` 立即识别为西文 |
| 关闭某个内置模式（仓颉+拼音混打） | `<schema>.custom.yaml` | `abc_segmentor/extra_tags` | 设为 `{}` 关闭 |
| 空码按空格清空 | `<schema>.custom.yaml` | `key_binder/bindings` + `translator/enable_sentence: false` | 见 customization-guide 示例 |

按键名称参考：[X11 keysymdef](https://github.com/rime/librime/blob/master/include/X11/keysymdef.h)。修饰符：`Shift`、`Control`、`Alt`、`Release`（按键弹起），按 `+` 连接。

## 四、标点符号

| 想做的事 | 目标文件 | 配置路径 | 说明 |
| --- | --- | --- | --- |
| 全角标点 | `<schema>.custom.yaml` | `punctuator/full_shape` | map：`"/" : "、"` |
| 半角标点 | `<schema>.custom.yaml` | `punctuator/half_shape` | 同上 |
| 用一组预设标点（替换 import） | `<schema>.custom.yaml` | `punctuator/import_preset` | 字符串，如 `default` |
| 含 `/` `+` `=` 的标点键 | `<schema>.custom.yaml` | 不能用路径 `punctuator/half_shape/+`，要在 `punctuator/half_shape:` 下整体覆盖 | 见 customization-guide 末节 |

## 五、繁简 / 字形转换

| 想做的事 | 目标文件 | 配置路径 | 说明 |
| --- | --- | --- | --- |
| 默认输出简体 | `<schema>.custom.yaml` | `switches/@n/reset` | 找到 `simplification` 开关，将其 `reset` 设为 `1`（n 是它在 `switches` 列表里的下标）|
| 简化器组件参数 | `<schema>.custom.yaml` | `simplifier/option_name` / `simplifier/opencc_config` | 字符串。前者指定开关名，后者指定 OpenCC 配置文件如 `t2s.json` |
| 增加自定义繁简映射 | `~/Library/Rime/opencc/` 下放 OpenCC 词典 | — | OpenCC 体系 |

## 六、模糊音 / 拼写算法

| 想做的事 | 目标文件 | 配置路径 | 说明 |
| --- | --- | --- | --- |
| 拼音模糊音 | `luna_pinyin.custom.yaml` | `speller/algebra` | 列表。新版可用 `__patch: [pinyin:/zh_z_bufen, ...]` 启用内置规则 |
| 简拼 | `luna_pinyin.custom.yaml` | `speller/algebra` | `__patch: [pinyin:/abbreviation]` |
| 拼写纠错 | `luna_pinyin.custom.yaml` | `speller/algebra` | `__patch: [pinyin:/spelling_correction, pinyin:/key_correction]` |

详细的 `xlit / derive / abbrev / fuzz / erase` 运算子语法见 `spelling-algebra.md`。

## 七、反查

| 想做的事 | 目标文件 | 配置路径 | 说明 |
| --- | --- | --- | --- |
| 修改反查词典 | `<schema>.custom.yaml` | `reverse_lookup/dictionary` | 字符串，如 `jyutping` |
| 修改反查棱镜（拼写法） | `<schema>.custom.yaml` | `reverse_lookup/prism` | 字符串 |
| 反查提示文字 | `<schema>.custom.yaml` | `reverse_lookup/tips` | 字符串如 `〔粤拼〕` |
| 反查触发符号 | `<schema>.custom.yaml` | `recognizer/patterns/reverse_lookup` | 正则，关闭设为空字符串 |

## 八、应用程序特定行为 *(前端)*

| 想做的事 | 目标文件 | 配置路径 | 说明 |
| --- | --- | --- | --- |
| macOS 应用默认西文 | `squirrel.custom.yaml` | `app_options/<bundle_id>` | 如 `app_options/com.apple.Xcode/ascii_mode: true`。Bundle ID 见 `Info.plist` |
| macOS 应用启用 inline preedit | `squirrel.custom.yaml` | `app_options/<bundle_id>/inline_preedit: true` | 同上 |
| Windows 应用默认西文 | `weasel.custom.yaml` | `app_options/<exe>` | 程序名小写，如 `gvim.exe` |
| 取消应用的特殊设置 | 同上文件 | `app_options/<id>: {}` | 空 map 表示「无特殊设置」 |

## 九、外观（字体、配色） *(前端)*

仅小狼毫 Weasel 与鼠鬚管 Squirrel；iBus / fcitx5-Rime 不通过 Rime 控制外观。

### Weasel（Windows）

| 想做的事 | 路径（`weasel.custom.yaml`） | 备注 |
| --- | --- | --- |
| 字体 | `style/font_face` | 系统字体名，如 `"明兰"` |
| 字号 | `style/font_point` | 整数 |
| 横排候选 | `style/horizontal` | 布尔 |
| 内嵌编码（仅 TSF 模式） | `style/inline_preedit` | 布尔 |
| 显示托盘图标 | `style/display_tray_icon` | 布尔 |
| 选用配色方案 | `style/color_scheme` | 字符串，引用下面定义的标识 |
| 自定义配色 | `preset_color_schemes/<id>` | map，字段见 customization-guide「定制配色方案」 |

### Squirrel（macOS）

类似 `style/...` 与 `preset_color_schemes/...`，键名常见有 `font_face`、`font_point`、`horizontal`、`inline_preedit`、`color_scheme` 等。完整字段参见共享文件夹下 `squirrel.yaml`。

可视化生成配色：
- 小狼毫：<https://bennyyip.github.io/Rime-See-Me/>
- 鼠鬚管：<https://gjrobert.github.io/Rime-See-Me-squirrel/>

## 十、输入方案文件 (`*.schema.yaml`) 中的关键节点

如果你在写或读一个 `<id>.schema.yaml` 源文件，常见的顶级节点：

- `schema/` — 方案元信息：`schema_id`、`name`、`version`、`author`、`description`、`dependencies`
- `switches` — 状态开关列表（如 `ascii_mode`、`full_shape`、`simplification`、`zh_simp`），每项含 `name`、`reset`、`states`
- `engine/` — 处理流水线：`processors`、`segmentors`、`translators`、`filters` 四类组件名列表
- `speller/` — 拼写器：`alphabet`、`delimiter`、`algebra`、`initials`、`finals`、`max_code_length`
- `translator/` — 翻译器：`dictionary`、`prism`、`spelling_hints`、`enable_completion`、`enable_sentence`、`enable_user_dict`、`preedit_format`、`comment_format` …
- `punctuator/` — 标点：`import_preset`、`full_shape`、`half_shape`
- `key_binder/` — 按键绑定：`import_preset`、`bindings`
- `recognizer/` — 输入串识别：`import_preset`、`patterns/<name>`
- `simplifier/` / `filter`-类：`option_name`、`opencc_config`、`tips`
- `reverse_lookup/` — 反查器：`dictionary`、`prism`、`tips`、`preedit_format`、`enable_completion`

引擎流水线、各组件的语义与默认值，详见 `schemas.md` 内的「输入方案」章节。

## 十一、词典文件 (`*.dict.yaml`)

- 头部 YAML 配置（不支持编译指令）：
  - `name` — 字典 ID（必须与文件名一致）
  - `version` — 字符串如 `'1.0'`
  - `sort` — `by_weight` 或 `original`
  - `use_preset_vocabulary` — 是否加载预设词彙表
  - `import_tables` — 列表，从其他字典导入
  - `vocabulary` — 用于自定义最小字表的子集
  - `columns` — 自定义文本列顺序
- `...` 之后是文本词条，每行：`<词文>\t<拼写>[\t<频次>]`

---

## 通用补靪写法速查（详见 `configuration-syntax.md`）

```yaml
patch:
  "节点/路径": 新值                       # 替换
  "节点/路径/+": { 子键: 值 }             # map 合并到现有节点
  "节点/路径/+": [追加项]                 # list 追加
  "节点/路径/=": { 全新内容 }             # 整体替换 map（防止合并）
  "list_node/@0": 新值                    # 替换列表第 0 项
  "list_node/@last": 新值                 # 替换最后一项
  "list_node/@before 0": 插入新项         # 在第 0 项前插入（不推荐补靪中用）
  "list_node/@after last": 插入新项       # 末尾追加（同 @next）
  "list_node/@next": 插入新项

  # 引用另一节点的补靪
  "节点":
    __patch: 另一处的补靪节点名

  # 启用一组规则
  "speller/algebra":
    __patch:
      - pinyin:/zh_z_bufen
      - pinyin:/abbreviation
```

**重要：** 整份 `.custom.yaml` 文件中，`patch:` 只能写一次。后续修改要在同一个 `patch:` map 下继续添加 key。
