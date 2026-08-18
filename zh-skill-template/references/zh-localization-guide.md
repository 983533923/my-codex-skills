# 技能中文化指南（完整版）

本文是 `zh-skill-template` 的配套参考文档。当需要了解技能中文化的逐条规则、字段细节、编码注意事项、安装前汉化流程或排查问题时阅读。

## 一、技能结构总览

```
技能目录/
├── SKILL.md            # 必需：触发判断 + 主指令
├── agents/
│   └── openai.yaml     # 推荐：界面显示元数据（缺失时可新建）
├── references/         # 可选：按需加载的说明文档
├── scripts/            # 可选：可执行代码
└── assets/             # 可选：输出用资源
```

## 二、各文件的中文化规则

### 1. SKILL.md（核心）

- frontmatter 必须包含 `name` 和 `description`，且只能有这两个字段。
- `name`：保留英文，小写字母/数字/连字符，非空、单行、≤100 字符。
- `description`：非空、单行、≤500 字符。建议中英双语（中文触发 + 英文兜底）。
- 正文：可以全中文，模型能理解；代码块、命令、路径保持原样。

### 2. agents/openai.yaml（界面元数据）

**openai.yaml 是"推荐"而非"必需"文件**，只影响界面显示，不影响技能触发和功能。很多第三方技能没有它，汉化时可新建。

字段说明：

- `interface.display_name`：界面显示的名字，直接写成中文。
- `interface.short_description`：界面简介（25–74 字符），写成中文短句。
- `interface.default_prompt`：默认提示词，要显式提到 `$技能名`。
- `interface.icon_small` / `icon_large`：图标路径，**仅在技能已有 assets 图标时才写**，否则省略；路径不能改。
- 所有字符串值用英文双引号包裹。

**缺失时如何新建（没有源文件可翻译）：** 依据 `SKILL.md` 的 `description` 和正文、以及 GitHub 仓库的 `README.md`/描述，提炼创作出中文的 `display_name`、`short_description`、`default_prompt`。

示例（给 `find-skill` 新建）：

```yaml
interface:
  display_name: "技能查找器"
  short_description: "从 14 个来源按 GitHub 星标查找并安装代理技能"
  default_prompt: "使用 $find-skill 帮我查找一个 Docker 相关的技能"
```

### 3. references/（说明文档）

- 可以全中文；模型按需加载。
- 文档里的代码示例、命令、文件路径、API 端点保持原样。
- 超过 100 行建议开头加目录。

### 4. scripts/（可执行代码）

- **绝对不能把代码"翻译"成中文**——变量名、函数名、参数、命令改中文会导致脚本无法运行。
- 注释可以写成中文（不影响执行）。
- 脚本的用途说明写在 SKILL.md 里，用户看 SKILL.md 即可，不必读代码。

### 5. assets/（资源）

- 模板、图片、字体等，不涉及语言，无需改动。

## 三、编码与格式注意事项（最容易踩的坑）

1. **UTF-8 无 BOM**：SKILL.md 和 openai.yaml 保存为 UTF-8 带 BOM 会导致解析失败、技能无法加载（官方已知问题）。
2. **description 必须单行**：不要在里面换行。
3. **YAML 引号**：含冒号、引号等特殊字符的中文字符串要用英文双引号包裹。
4. **路径和键名**：`./assets/xxx.svg`、`interface:` 这类键名/路径永远保持英文原样。

## 四、安装技能前的汉化建议流程

当用户要求安装一个新技能时，本技能按三步执行：

1. **安装前检查该技能**：在动手安装前，先查看技能来源（GitHub 仓库结构 / 技能包内容），读取其 `SKILL.md` 的 `name` 和 `description`，并检查是否有 `agents/openai.yaml`，判断是否为英文/外文、是否值得汉化。
2. **给出汉化建议**：对照本指南第二节的规则，给出该技能"能改/不能改"清单，给出 description 的中英双语改写示例；说明 openai.yaml 已有则汉化界面字段、缺失则新建（依据 SKILL.md 和仓库信息创作）。
3. **询问是否在安装过程中汉化**：询问用户是否在安装该技能的同时进行汉化；**用户确认后才安装并修改**技能文件（含补齐 openai.yaml），修改后运行 `quick_validate.py` 校验。

## 五、校验与测试

1. 校验格式：
   ```
   python <skill-creator>/scripts/quick_validate.py <技能目录>
   ```
2. 重启 Codex（或新开任务）。
3. 用中文提示词测试自动触发，或用 `$技能名` / `/use 技能名` 测试显式触发。
