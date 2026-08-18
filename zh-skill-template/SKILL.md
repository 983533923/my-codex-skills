---
name: zh-skill-template
description: "中文技能模板与技能中文化指南。Use when creating a new Codex skill written in Chinese, localizing an existing skill's files into Chinese, needing a bilingual skill template, or before installing a skill from a GitHub repo URL to review it, propose Chinese localization, and ask whether to localize it during installation. 当用户需要创建中文技能、把技能内容改成中文、参考中文技能写法、要求安装 GitHub 上的技能（安装前检查该技能并给出汉化建议、询问是否在安装过程中汉化）、或询问刚安装的技能能否汉化时使用。"
---

# 中文技能模板（zh-skill-template）

## 这个技能是什么

这是一个**完整的中文技能范例**，同时是一份**技能中文化指南**。它演示了：

- SKILL.md 正文如何用中文书写；
- frontmatter 的 `description` 如何写成中英双语，让中文提示词也能准确触发；
- `agents/openai.yaml` 的界面字段如何用中文（缺失时如何依据 SKILL.md 和仓库信息新建）；
- `references/` 文档如何用中文；
- 哪些文件**不能**改成中文（scripts/ 代码、路径、键名等）。

当你需要新建一个中文技能、把已有技能改成中文、或要求安装一个外文技能想知道能否边装边汉化时，直接参考本技能的结构和规则。

## 中文化规则速查

| 文件 / 字段 | 能否用中文 | 说明 |
| --- | --- | --- |
| `SKILL.md` 正文 | ✅ 可以 | 模型能读中文，正文全中文不影响使用 |
| `name`（frontmatter） | ❌ 保留英文 | 用作 /use 命令和系统标识，只能小写字母/数字/连字符 |
| `description`（frontmatter） | ✅ 中英双语 | 这是触发机制的核心，双语让中英文提示词都能匹配 |
| `agents/openai.yaml` 界面字段 | ✅ 推荐中文，**缺失则新建** | display_name / short_description / default_prompt 是给人看的；没有该文件时按下方"创建汉化的 agents/openai.yaml"新建 |
| `references/` 文档 | ✅ 可以 | 只改文字；代码块、命令、路径保持原样 |
| `scripts/` 代码 | ❌ 不能翻译 | 会被实际执行，翻译会破坏功能；最多加中文注释 |
| `assets/` 资源 | — 不涉及 | 图片、模板、字体，无语言问题 |
| YAML 键名 / 文件路径 | ❌ 不能改 | 改了就解析失败或引用失效 |

## 触发说明（重要）

本技能的自动触发**不能 100% 保证**：当用户只说"安装 xxx 技能"时，Codex 通常只会加载安装类技能（如 skill-installer），不会自动再加载本技能。

**要确保触发，请在安装请求中显式提到本技能**，例如：

> 用 skill-installer 从 GitHub 仓库 xxx/yyy 安装技能，**安装前用 $zh-skill-template 检查该技能并给出汉化建议**

或者在安装前单独说"这个技能能汉化吗，我想边装边汉化"，即可触发本流程。

## 安装技能前的汉化建议流程

当用户**要求安装一个新技能**（例如给出 GitHub 仓库地址、用 skill-installer 安装、或手动下载）时，按以下流程执行：

1. **安装前检查该技能**：在动手安装之前，先查看技能来源（GitHub 仓库结构 / 技能包内容），读取其 `SKILL.md` 的 `name` 和 `description`，并**检查是否有 `agents/openai.yaml`**，判断是否为英文/外文、是否值得汉化。
2. **给出汉化建议**：对照上方"中文化规则速查"，针对该技能给出具体建议：
   - 哪些文件可以汉化（SKILL.md 正文、references/ 的文字部分）；
   - 哪些不能动（name、scripts/ 代码、文件路径、YAML 键名、assets/）；
   - `agents/openai.yaml`：已有则建议汉化其界面字段；**缺失则说明需要新建**（依据 SKILL.md 内容和 GitHub 仓库信息创作中文版）；
   - description 建议改成中英双语，并给出针对该技能的改写示例。
3. **询问是否在安装过程中汉化**：问用户"是否要在安装该技能的过程中同时进行汉化？"——**未经用户确认，不要安装或修改任何文件**。用户确认后：先完成安装，再按下方"如何用本模板创建新技能"的流程执行汉化（**含检查/创建 `agents/openai.yaml`**），并用 `quick_validate.py` 校验。

**示例**：用户说"安装 https://github.com/xxx/yyy 这个技能"→ 先检查该仓库的 SKILL.md 和是否带 agents/openai.yaml，给出汉化建议，然后询问是否在安装过程中汉化；确认后先安装、再汉化并补齐 openai.yaml。

## 创建汉化的 agents/openai.yaml

很多技能（尤其是第三方仓库的技能）**没有 `agents/openai.yaml` 文件**。为了让界面显示中文技能名和简介，汉化时需要新建它。

**没有源文件可翻译时，依据以下内容创作：**

- `SKILL.md` 的 `description` 和正文（技能做什么、何时用）；
- GitHub 仓库的 `README.md` 或仓库描述（项目定位、功能亮点）。

**字段写法：**

| 字段 | 写法 |
| --- | --- |
| `interface.display_name` | 单技能用中文名；**多技能仓库用三段式：`仓库总名｜类别前缀｜技能中文名`**（见下方说明） |
| `interface.short_description` | 中文简介（约 25–74 字符），一句话说明功能和适用场景 |
| `interface.default_prompt` | 中文默认提示词，必须提到 `$技能名`（用该技能的 name） |
| `icon_small` / `icon_large` | 仅在技能目录已有 assets 图标时才写，否则省略 |

**模板：**

```yaml
interface:
  display_name: "技能中文名"
  short_description: "一句话中文简介（功能 + 适用场景）"
  default_prompt: "使用 $技能name 帮我……（中文示例提示词）"
```

**多技能仓库的 display_name 命名：**

当一次安装的多个技能来自同一个仓库、且分属多个类别时，`display_name` 用三段式，让界面一眼区分来源和类别：

```
仓库总名｜类别前缀｜技能中文名
```

- **仓库总名**：依据安装来源（GitHub 仓库名、README、仓库描述）翻译成中文全称，例如 jnMetaCode/superpowers-zh → "AI 编程超能力"；
- **类别前缀**：统一 2 字词（入门 / 规划 / 开发 / 测试 / 调试 / 审查 / 质量 / 技能 / 文档 / 规范 / 工作流），判断依据依次为 `metadata.hermes.tags`、SKILL.md 的 `description` 和正文、仓库 categories 目录结构；
- **技能中文名**：该技能的中文名。

示例：`AI 编程超能力｜调试｜系统化调试`。

注意：键名（`interface:`、`display_name:` 等）保持英文，字符串值用英文双引号包裹，文件保存为 UTF-8 无 BOM。

## 如何用本模板创建新技能

1. **初始化**：运行 skill-creator 的初始化脚本生成新技能目录：
   ```
   python <skill-creator>/scripts/init_skill.py <英文技能名> --path ~/.codex/skills
   ```
2. **写 description**：按下方"触发词写法"填中英双语描述。
3. **写中文正文**：把指令写成中文，结构参考本文件。
4. **创建或更新 agents/openai.yaml**：没有该文件就按上方"创建汉化的 agents/openai.yaml"新建中文版；已有则把 display_name、short_description、default_prompt 汉化。
5. **校验**：运行 quick_validate.py 检查格式。
6. **测试**：重启 Codex，用中文提示词验证能否触发。

## 触发词写法（description）

`description` 是技能自动触发的核心，Codex 靠它判断何时加载技能。推荐写法：

- 中文部分写清"做什么 + 何时用"，覆盖你的常用说法；
- 英文部分用 "Use when ..." 句式覆盖英文场景；
- 整段保持**一行**，不超过 500 字符。

示例（就是本技能的写法）：

```text
中文技能模板与技能中文化指南。Use when creating a new Codex skill written in Chinese, localizing an existing skill's files into Chinese, needing a bilingual skill template, or before installing a skill from a GitHub repo URL to review it, propose Chinese localization, and ask whether to localize it during installation. 当用户需要创建中文技能、把技能内容改成中文、参考中文技能写法、要求安装 GitHub 上的技能（安装前检查该技能并给出汉化建议、询问是否在安装过程中汉化）、或询问刚安装的技能能否汉化时使用。
```

## 显式触发方式

- **自动触发**：在对话中描述你的需求（如"帮我创建一个中文技能""我想安装一个技能，能边装边汉化吗"），匹配到 description 后自动加载；
- **显式触发**：在提示词里写 `$zh-skill-template`，或使用 `/use zh-skill-template`。

## 详细指南

中文化规则的完整说明（含 openai.yaml 每个字段的写法、缺失时如何新建、编码注意事项、校验步骤、安装前汉化流程）见：

- [zh-localization-guide.md](references/zh-localization-guide.md) —— 当需要逐条规则、字段细节或排查问题时阅读。
