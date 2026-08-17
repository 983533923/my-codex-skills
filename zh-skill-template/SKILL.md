---
name: zh-skill-template
description: "中文技能模板与技能中文化指南。Use when creating a new Codex skill written in Chinese, localizing an existing skill's files into Chinese, or needing a bilingual skill template (Chinese body, bilingual description, Chinese UI metadata). Also use right after the user installs a skill to review it and propose Chinese localization. 当用户需要创建中文技能、把技能内容改成中文、参考中文技能写法、安装新技能或询问刚安装的技能是否需要汉化时使用。"
---

# 中文技能模板（zh-skill-template）

## 这个技能是什么

这是一个**完整的中文技能范例**，同时是一份**技能中文化指南**。它演示了：

- SKILL.md 正文如何用中文书写；
- frontmatter 的 `description` 如何写成中英双语，让中文提示词也能准确触发；
- `agents/openai.yaml` 的界面字段如何用中文；
- `references/` 文档如何用中文；
- 哪些文件**不能**改成中文（scripts/ 代码、路径、键名等）。

当你需要新建一个中文技能、把已有技能改成中文、或刚安装了一个技能想知道它能否汉化时，直接参考本技能的结构和规则。

## 中文化规则速查

| 文件 / 字段 | 能否用中文 | 说明 |
| --- | --- | --- |
| `SKILL.md` 正文 | ✅ 可以 | 模型能读中文，正文全中文不影响使用 |
| `name`（frontmatter） | ❌ 保留英文 | 用作 /use 命令和系统标识，只能小写字母/数字/连字符 |
| `description`（frontmatter） | ✅ 中英双语 | 这是触发机制的核心，双语让中英文提示词都能匹配 |
| `agents/openai.yaml` 界面字段 | ✅ 推荐中文 | display_name / short_description / default_prompt 是给人看的 |
| `references/` 文档 | ✅ 可以 | 只改文字；代码块、命令、路径保持原样 |
| `scripts/` 代码 | ❌ 不能翻译 | 会被实际执行，翻译会破坏功能；最多加中文注释 |
| `assets/` 资源 | — 不涉及 | 图片、模板、字体，无语言问题 |
| YAML 键名 / 文件路径 | ❌ 不能改 | 改了就解析失败或引用失效 |

## 安装技能后的汉化建议流程

当用户**安装了一个新技能**（例如用 skill-installer 安装、手动放入 `~/.codex/skills`、或通过插件安装）时，按以下流程执行：

1. **查询安装的技能**：检查 `~/.codex/skills`（含 `.system/`）以及插件技能缓存目录，找出最近安装或新增的技能，读取其 `SKILL.md` 的 `name` 和 `description`，判断当前是否为英文、是否值得汉化。
2. **给出汉化建议**：对照上方"中文化规则速查"，针对该技能给出具体建议：
   - 哪些文件可以汉化（SKILL.md 正文、openai.yaml 界面字段、references/ 的文字部分）；
   - 哪些不能动（name、scripts/ 代码、文件路径、YAML 键名、assets/）；
   - description 建议改成中英双语，并给出针对该技能的改写示例。
3. **询问是否汉化**：问用户"是否要使用本技能（zh-skill-template）对该技能进行汉化？"——**未经用户确认，不要直接修改已安装的技能文件**。用户确认后，按下方"如何用本模板创建新技能"的流程执行汉化，并用 `quick_validate.py` 校验。

**示例**：用户说"我刚用 skill-installer 安装了 xx 技能"→ 先定位该技能目录，读取其 SKILL.md，给出汉化建议，然后询问是否使用本技能汉化。

## 如何用本模板创建新技能

1. **初始化**：运行 skill-creator 的初始化脚本生成新技能目录：
   ```
   python <skill-creator>/scripts/init_skill.py <英文技能名> --path ~/.codex/skills
   ```
2. **写 description**：按下方"触发词写法"填中英双语描述。
3. **写中文正文**：把指令写成中文，结构参考本文件。
4. **更新 openai.yaml**：把 display_name、short_description、default_prompt 写成中文。
5. **校验**：运行 quick_validate.py 检查格式。
6. **测试**：重启 Codex，用中文提示词验证能否触发。

## 触发词写法（description）

`description` 是技能自动触发的核心，Codex 靠它判断何时加载技能。推荐写法：

- 中文部分写清"做什么 + 何时用"，覆盖你的常用说法；
- 英文部分用 "Use when ..." 句式覆盖英文场景；
- 整段保持**一行**，不超过 500 字符。

示例（就是本技能的写法）：

```text
中文技能模板与技能中文化指南。Use when creating a new Codex skill written in Chinese, localizing an existing skill's files into Chinese, or needing a bilingual skill template. Also use right after the user installs a skill to review it and propose Chinese localization. 当用户需要创建中文技能、把技能内容改成中文、参考中文技能写法、安装新技能或询问刚安装的技能是否需要汉化时使用。
```

## 显式触发方式

- **自动触发**：在对话中描述你的需求（如"帮我创建一个中文技能""我刚安装了一个技能，能汉化吗"），匹配到 description 后自动加载；
- **显式触发**：在提示词里写 `$zh-skill-template`，或使用 `/use zh-skill-template`。

## 详细指南

中文化规则的完整说明（含 openai.yaml 每个字段的写法、编码注意事项、校验步骤、安装后汉化流程）见：

- [zh-localization-guide.md](references/zh-localization-guide.md) —— 当需要逐条规则、字段细节或排查问题时阅读。
