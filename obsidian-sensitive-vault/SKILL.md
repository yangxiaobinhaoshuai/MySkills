---
name: obsidian-sensitive-vault
description: Manage the user's personal Obsidian vault as a search-first sensitive-records system. Use whenever the user asks to organize, simplify, migrate, review, or maintain their Obsidian vault that stores passwords, tokens, subscriptions, account records, screenshots, or other private service data. Prefer the user's current workflow: root-level Inbox.md as the default input layer, Chinese descriptions, low directory depth, minimal classification, and agent-assisted cleanup into Private/Groups only when Inbox becomes noisy.
---

# Obsidian Sensitive Vault Skill

用于维护用户的个人 Obsidian 敏感信息库。这个 skill 只适用于该用户当前这套偏好，不适合泛化成通用知识管理方案。

## 核心偏好

- 默认录入入口是根目录 `Inbox.md`。
- 用户新增内容时，优先直接追加到 `Inbox.md`，不要要求先分类。
- 搜索优先，不依赖目录浏览。
- 说明、迁移备注、规则文档优先使用中文。
- `Private/Groups/` 是整理层，不是默认输入层。
- 目录层级尽量浅，避免超过 3 到 4 层。
- 敏感值继续保存在 vault 内。
- 用户习惯持续追加；agent 负责后续整理、归并、降噪和拆卷。

## 目录约定

优先保持这些入口在根目录可见：

- `Inbox.md`
- `Dashboards.md`
- `Vault_Index.md`
- `Vault_Maintenance.md`
- `AGENTS.md`
- `README.md`

敏感内容层：

- `Private/Groups/`：整理后的 group 分卷
- `Private/Attachments/`：敏感截图
- `Private/Archive/`：原件和历史
- `Private/Credentials/`：旧结构，按需迁移

## 工作方式

当用户要求整理 vault 时，按这个顺序处理：

1. 先判断这是录入、整理、重构、还是 review。
2. 如果是新增记录，优先写入 `Inbox.md`，不要强推复杂结构。
3. 如果是整理，优先压低层级、减少分类、减少搜索噪音。
4. 只有在 `Inbox.md` 已经明显变乱时，才把内容归并到 `Private/Groups/`。
5. 修改后检查 wiki link 和图片 embed，避免断链。

## Group 规则

只使用少量 group：

- `core`
- `service`
- `infra`
- `finance`
- `school`
- `misc`

分卷命名使用：

- `core_01.md`
- `service_01.md`
- `infra_02.md`

不要使用：

- `acc1.md`
- `acc2.md`
- 无语义编号文件

## 记录格式

在 `Inbox.md` 中追加记录时，优先使用简短块结构：

```md
## 服务名
- aliases:
- tags:
- login:
- email:
- password:
- token:
- recovery:
- urls:
- notes:
- updated:
```

允许更随手的追加，只要服务名和关键信息可搜索；不要为了格式完美阻塞录入。

## 安全规则

- 不要在对话里打印、总结或复制密码、token、API key、恢复码、订阅链接、支付信息等敏感内容。
- 整理敏感文件前，先把原始文件复制到 `Private/Archive/Originals/<YYYY-MM-DD>/`。
- 优先移动和链接，不要直接删除。
- 敏感截图放到 `Private/Attachments/`；非敏感图片放到 `Attachments/Images/`。
- 不要把 `Private/` 加进 `.gitignore`，除非用户明确改变同步策略。

## 不要做的事

- 不要恢复成“大而全”的单一总表。
- 不要强推细分类目录。
- 不要把 `Groups` 当成默认输入层。
- 不要让说明文档散落在很深的目录里。
- 不要为了“结构整齐”一次性全量重写所有敏感记录。

## 何时触发

这个 skill 应在以下请求中优先使用：

- 用户要求整理、重构、简化、review 这个 Obsidian vault
- 用户要求降低目录层级或分类负担
- 用户要求改 `Inbox.md`、`Vault_Index.md`、`Vault_Maintenance.md`、`Dashboards.md`、`AGENTS.md`、`README.md`
- 用户要求把敏感记录迁移到更适合搜索和 agent 整理的结构
