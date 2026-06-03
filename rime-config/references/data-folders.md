# Rime 数据文件夹与部署

Rime 在两类目录中查找配置：**用户文件夹**（你写自定义补靪的地方）和**共享文件夹**（默认配置与预设输入方案）。查找资源时，**先用户后共享** —— 同名文件中，用户文件夹的版本胜出。

## 用户文件夹（你要编辑的地方）

按前端分平台：

| 前端 / 平台 | 用户文件夹路径 |
| --- | --- |
| 鼠鬚管 Squirrel（macOS） | `~/Library/Rime/` |
| 小狼毫 Weasel（Windows） | `%APPDATA%\Rime\` |
| 鼠鬚管菜单 → 「用户设定…」 | 直接打开上述目录 |
| iBus-Rime（Linux） | `~/.config/ibus/rime/` |
| fcitx4-Rime（Linux） | `~/.config/fcitx/rime/` |
| fcitx5-Rime（Linux） | `~/.local/share/fcitx5/rime/` |

**重要：** 务必使用绝对路径，librime 不接受相对路径。

### 用户文件夹里有什么

用家自己创建/下载的文件：

- `<config>.custom.yaml` — 对配置文件 `<config>.yaml` 或 `<config>.schema.yaml` 的**补靪**（绝大多数定制工作就发生在这里）。常见的有：
  - `default.custom.yaml` — 全局通用设置（方案选单、快捷键、共享开关）
  - `squirrel.custom.yaml` — macOS 鼠鬚管前端设置（字体、配色、外观、`app_options`）
  - `weasel.custom.yaml` — Windows 小狼毫前端设置（字体、配色、托盘等）
  - `ibus_rime.custom.yaml` — iBus-Rime 前端设置（**不含** 字体配色等外观项）
  - `<schema_id>.custom.yaml` — 单个输入方案的定制，如 `luna_pinyin.custom.yaml`、`cangjie5.custom.yaml`
- `<schema_id>.schema.yaml` — 自己下载或编写的输入方案。
- `<dict_id>.dict.yaml` — 自己下载或编写的韵书（词典）。
- `<name>.txt` — 纯文本词典，如自定义词组。
- `opencc/*` — OpenCC 繁简转换配置与字典。

输入法运行时写入的文件（一般不要手动修改）：

- `<schema>.userdb/` — 用户词典（输入习惯、字频）。
- `installation.yaml` — 安装信息。
- `user.yaml` — 当前选中的输入方案、各开关状态等。

部署期间生成的目录：

- `build/` — 编译后的机读配置缓存。**调试时可以查看**这里的 YAML，确认补靪是否生效。
- `trash/` — Rime 升级时被废弃的旧文件，用家确认无误后可删除。

注：librime ≥ 1.3，`build/` 取代了过去散布于用户文件夹根下的编译产物。

## 共享文件夹（默认配置，**只读参考**）

存放默认的输入方案、韻書和默认配置。**通常不要直接编辑** —— 升级会被覆盖。要修改其中任何内容，请在用户文件夹建一份对应的 `.custom.yaml` 补靪。

| 前端 | 共享文件夹路径 |
| --- | --- |
| 鼠鬚管 Squirrel | `/Library/Input Methods/Squirrel.app/Contents/SharedSupport/` |
| 小狼毫 Weasel | `<安装目录>\data\` |
| iBus-Rime / fcitx-Rime | `/usr/share/rime-data/` |

如果想知道某个设置项的默认值或上下文（再写补靪覆盖），到共享文件夹查 `default.yaml`、`<schema_id>.schema.yaml`、`weasel.yaml` / `squirrel.yaml` 等源文件。

## 重新部署（让改动生效）

每次修改 `.custom.yaml`，都需要**重新部署**才会生效。各前端的操作方法：

| 前端 | 重新部署的方法 |
| --- | --- |
| 鼠鬚管 Squirrel | 系统输入法菜单 → 「重新部署」 |
| 小狼毫 Weasel | 开始菜单 → 小狼毫输入法 → 重新部署；或右键托盘 → 重新部署 |
| 中州韵（通用） | 点击输入法状态栏上的 ⟲ (Deploy) 按钮 |
| iBus-Rime | 终端运行 `touch ~/.config/ibus/rime/ && ibus restart` |
| fcitx5-Rime | 右键 fcitx5 托盘 → Rime → 重新部署；或 `fcitx5 -r` 重启 |

**部署时间**：编译输入方案需要数秒到数十秒。期间若打不出中文，等一会儿。

**部署后验证**：
1. 如方案未生效，按 `Ctrl+`​ \`​ `（或 F4）唤出方案选单，确认目标方案出现且可选。
2. 仍异常？查看 `~/.config/...../rime.<前端>.INFO` 日志（macOS 在 `/tmp/`），找首字符为 `E` 的行定位错误。
3. 还可以对照 `build/` 下的编译结果文件，确认补靪有没有合入。

## 调试日志位置

| 前端 | 日志位置 |
| --- | --- |
| 鼠鬚管 Squirrel | `/tmp/rime.squirrel.*.log`（启动时新建，重启后旧文件可能被清） |
| 小狼毫 Weasel | `%TEMP%\rime.weasel.*.log` |
| iBus / fcitx-Rime | `/tmp/rime.ibus.*.log` / `/tmp/rime.fcitx.*.log` |

日志按级别分 `INFO`、`WARNING`、`ERROR`。优先看 `ERROR` 与 `INFO` 里行首为 `E` 的记录。
