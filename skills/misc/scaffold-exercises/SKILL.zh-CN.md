---
name: scaffold-exercises
description: 创建通过 linting 的练习目录结构，包含章节、问题、解决方案和解释器。当用户想要脚手架练习、创建练习存根或设置新课程章节时使用。
---

# 脚手架练习

创建通过 `pnpm ai-hero-cli internal lint` 的练习目录结构，然后用 `git commit` 提交。

## 目录命名

- **章节**：在 `exercises/` 内的 `XX-section-name/`（例如 `01-retrieval-skill-building`）
- **练习**：章节内的 `XX.YY-exercise-name/`（例如 `01.03-retrieval-with-bm25`）
- 章节编号 = `XX`，练习编号 = `XX.YY`
- 名称用 dash-case（小写，连字符）

## 练习变体

每个练习至少需要这些子文件夹之一：

- `problem/` - 带 TODO 的学生工作区
- `solution/` - 参考实现
- `explainer/` - 概念材料，无 TODO

创建存根时，默认使用 `explainer/`，除非计划另有规定。

## 必需文件

每个子文件夹（`problem/`、`solution/`、`explainer/`）需要一个 `readme.md`，该文件：

- **不为空**（必须有真实内容，即使单个标题行也可以）
- 没有断开的链接

创建存根时，用标题和描述创建最少的 readme：

```md
# 练习标题

此处为描述
```

如果子文件夹有代码，它也需要一个 `main.ts`（>1 行）。但对于存根，只读 readme 的练习是可以的。

## 工作流

1. **解析计划** - 提取章节名称、练习名称和变体类型
2. **创建目录** - 每个路径的 `mkdir -p`
3. **创建存根 readme** - 每个变体文件夹一个 `readme.md`，包含标题
4. **运行 lint** - `pnpm ai-hero-cli internal lint` 进行验证
5. **修复任何错误** - 迭代直到 lint 通过

## Lint 规则摘要

Linter（`pnpm ai-hero-cli internal lint`）检查：

- 每个练习有子文件夹（`problem/`、`solution/`、`explainer/`）
- `problem/`、`explainer/` 或 `explainer.1/` 中至少有一个存在
- 主子文件夹中的 `readme.md` 存在且非空
- 没有 `.gitkeep` 文件
- 没有 `speaker-notes.md` 文件
- readme 中没有断开的链接
- readme 中没有 `pnpm run exercise` 命令
- 每个子文件夹除非是只读 readme 的都需要 `main.ts`

## 移动/重命名练习

重新编号或移动练习时：

1. 使用 `git mv`（不是 `mv`）重命名目录——保留 git 历史
2. 更新数字前缀以维持顺序
3. 移动后重新运行 lint

示例：

```bash
git mv exercises/01-retrieval/01.03-embeddings exercises/01-retrieval/01.04-embeddings
```

## 示例：从计划创建存根

给定这样的计划：

```
Section 05: Memory Skill Building
- 05.01 Introduction to Memory
- 05.02 Short-term Memory (explainer + problem + solution)
- 05.03 Long-term Memory
```

创建：

```bash
mkdir -p exercises/05-memory-skill-building/05.01-introduction-to-memory/explainer
mkdir -p exercises/05-memory-skill-building/05.02-short-term-memory/{explainer,problem,solution}
mkdir -p exercises/05-memory-skill-building/05.03-long-term-memory/explainer
```

然后创建 readme 存根：

```
exercises/05-memory-skill-building/05.01-introduction-to-memory/explainer/readme.md -> "# Introduction to Memory"
exercises/05-memory-skill-building/05.02-short-term-memory/explainer/readme.md -> "# Short-term Memory"
exercises/05-memory-skill-building/05.02-short-term-memory/problem/readme.md -> "# Short-term Memory"
exercises/05-memory-skill-building/05.02-short-term-memory/solution/readme.md -> "# Short-term Memory"
exercises/05-memory-skill-building/05.03-long-term-memory/explainer/readme.md -> "# Long-term Memory"
```
