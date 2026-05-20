---
name: write-a-skill
description: 使用正确的结构、渐进式披露和捆绑资源创建新的 Agent 技能。当用户想要创建、编写或构建新技能时使用。
---

# 编写技能

## 流程

1. **收集需求** - 询问用户：
   - 技能涵盖什么任务/领域？
   - 它应该处理哪些特定用例？
   - 它需要可执行脚本还是只需要说明？
   - 有任何要包含的参考材料吗？

2. **起草技能** - 创建：
   - SKILL.md，简洁说明
   - 如果内容超过 500 行，则使用额外的参考文件
   - 如果需要确定性操作，则使用实用脚本

3. **与用户一起审查** - 呈现草稿并询问：
   - 这涵盖你的用例吗？
   - 有什么遗漏或不清楚的吗？
   - 任何部分应该更详细/更简洁吗？

## 技能结构

```
skill-name/
├── SKILL.md           # 主说明（必需）
├── REFERENCE.md       # 详细文档（如需要）
├── EXAMPLES.md        # 使用示例（如需要）
└── scripts/           # 实用脚本（如需要）
    └── helper.js
```

## SKILL.md 模板

```md
---
name: skill-name
description: 能力简述。使用时机：[特定触发器]。
---

# Skill Name

## 快速开始

[最小工作示例]

## 工作流

[复杂任务的逐步流程和检查清单]

## 高级功能

[链接到单独文件：见 [REFERENCE.md](REFERENCE.md)]
```

## 描述要求

描述是**你的 Agent 在决定加载哪个技能时看到的唯一内容**。它与所有其他已安装技能一起呈现在系统提示中。你的 Agent 读取这些描述，并根据用户请求选择相关技能。

**目标**：给你的 Agent 足够的信息以了解：

1. 此技能提供什么能力
2. 何时/为何触发它（特定关键词、上下文、文件类型）

**格式**：

- 最多 1024 个字符
- 用第三人称写作
- 第一句：它做什么
- 第二句："Use when [特定触发器]"

**好示例**：

```
Extract text and tables from PDF files, fill forms, merge documents. Use when working with PDF files or when user mentions PDFs, forms, or document extraction.
```

**坏示例**：

```
Helps with documents.
```

坏示例让你的 Agent 无法将其与其他文档技能区分开来。

## 何时添加脚本

在以下情况下添加实用脚本：

- 操作是确定性的（验证、格式化）
- 相同的代码会被重复生成
- 错误需要明确处理

脚本比生成的代码节省 tokens 并提高可靠性。

## 何时拆分文件

在以下情况下拆分为单独的文件：

- SKILL.md 超过 100 行
- 内容有不同的领域（finance vs sales schemas）
- 高级功能很少需要

## 审查检查清单

起草后验证：

- [ ] 描述包含触发器（"Use when..."）
- [ ] SKILL.md 在 100 行以内
- [ ] 没有时间敏感信息
- [ ] 术语一致
- [ ] 包含具体示例
- [ ] 引用一级深度
