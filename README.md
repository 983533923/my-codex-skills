# my-codex-skills

我的 Codex 技能集合（中文优先）。

## 技能列表

### zh-skill-template — 中文技能模板与中文化指南

一个完整的中文技能范例，同时是一份技能中文化指南。

**自动触发场景：**

- 用户需要创建中文技能；
- 用户想把已有技能的内容改成中文；
- 用户安装新技能后，自动给出汉化建议，并询问是否使用本技能汉化。

**文件结构：**

```
zh-skill-template/
├── SKILL.md                            # 技能主文件（中文正文 + 中英双语触发描述）
├── agents/openai.yaml                  # 界面元数据（中文显示名 / 简介 / 默认提示词）
└── references/
    └── zh-localization-guide.md        # 技能中文化详细指南
```

## 安装方法

在 Codex 中直接说：

> 用 skill-installer 从 GitHub 仓库 983533923/my-codex-skills 安装 zh-skill-template 技能

或者手动把 `zh-skill-template` 文件夹放入 `~/.codex/skills/`（Windows 为 `C:\Users\<用户名>\.codex\skills\`），重启 Codex 即可使用。

## 维护说明

- 修改技能文件后，请同步更新本仓库，其他设备重新安装即可获取最新版；
- 技能内容保持 UTF-8 无 BOM 编码，description 保持单行且不超过 500 字符。
