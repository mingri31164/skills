---
name: setup-matt-pocock-skills
description: 在 AGENTS.md/CLAUDE.md 中设置 `## Agent skills` 块和 `docs/agents/`，以便工程技能知道这个仓库的 Issue 追踪器（GitHub 或本地 markdown）、分类标签词汇表和领域文档布局。在首次使用 `to-issues`、`to-prd`、`triage`、`diagnose`、`tdd`、`improve-codebase-architecture` 或 `zoom-out` 之前运行——或者如果这些技能似乎缺少关于 Issue 追踪器、分类标签或领域文档的上下文。
disable-model-invocation: true
---

# 设置 Matt Pocock 的技能

脚手架工程技能假设的每个仓库配置：

- **Issue 追踪器** — Issue 所在位置（默认 GitHub；也原生支持本地 markdown）
- **分类标签** — 用于五个规范分类角色的字符串
- **领域文档** — `CONTEXT.md` 和 ADR 所在位置，以及读取它们的消费者规则

这是一个提示驱动的技能，不是确定性脚本。探索，呈现你发现的，确认用户，然后写入。

## 流程

### 1. 探索

查看当前仓库以了解其起始状态。阅读存在的任何内容；不要假设：

- `git remote -v` 和 `.git/config` — 这是 GitHub 仓库吗？哪一个？
- `AGENTS.md` 和 `CLAUDE.md` 在仓库根目录 — 任一存在吗？任一已有 `## Agent skills` 部分吗？
- 根目录的 `CONTEXT.md` 和 `CONTEXT-MAP.md`
- `docs/adr/` 和任何 `src/*/docs/adr/` 目录
- `docs/agents/` — 此技能的先前输出已存在吗？
- `.scratch/` — 表明已使用本地-markdown Issue 追踪器约定

### 2. 呈现发现并询问

总结存在和缺失的内容。然后一次一个地引导用户完成三个决策——呈现一个部分，获得用户答案，然后进入下一个。不要一次倾倒所有三个。

假设用户不知道这些术语是什么意思。每个部分以简短解释开始（它是什么，为什么这些技能需要它，选择不同会发生什么变化）。然后展示选项和默认值。

**部分 A — Issue 追踪器。**

> 解释器："Issue 追踪器"是此仓库 Issue 所在的位置。像 `to-issues`、`triage`、`to-prd` 和 `qa` 这样的技能从那里读取并写入——它们需要知道是调用 `gh issue create`、在 `.scratch/` 下写一个 markdown 文件，还是遵循你描述的其他工作流。选择你实际为此仓库跟踪工作的位置。

默认态度：这些技能为 GitHub 设计。如果 `git remote` 指向 GitHub，建议那个。如果 `git remote` 指向 GitLab（`gitlab.com` 或自托管主机），建议 GitLab。否则（或如果用户偏好），提供：

- **GitHub** — Issue 位于仓库的 GitHub Issues（使用 `gh` CLI）
- **GitLab** — Issue 位于仓库的 GitLab Issues（使用 [`glab`](https://gitlab.com/gitlab-org/cli) CLI）
- **本地 markdown** — Issue 作为文件存在于 `.scratch/<feature>/` 下（适合 solo 项目或没有远程的仓库）
- **其他**（Jira、Linear 等）— 要求用户用一段话描述工作流；技能会将其记录为自由格式散文

**部分 B — 分类标签词汇表。**

> 解释器：当 `triage` 技能处理传入 Issue 时，它会通过状态机移动它——需要评估、等待报告者、准备好供 AFK Agent 拾取、准备好人工、或不会修复。为此，它需要应用与你实际配置的字符串匹配的标签（或者在 Issue 追踪器中等效的东西）。如果你的仓库已使用不同的标签名称（例如 `bug:triage` 而不是 `needs-triage`），在此处映射它们，以便技能应用正确的标签而不是创建重复。

五个规范角色：

- `needs-triage` — 维护者需要评估
- `needs-info` — 等待报告者
- `ready-for-agent` — 完全指定，AFK 就绪（Agent 可以在没有人上下文的情况下拾取它）
- `ready-for-human` — 需要人工实现
- `wontfix` — 将不会被处理

默认值：每个角色的字符串等于其名称。询问用户是否要覆盖任何。如果他们的 Issue 追踪器没有现有标签，默认值就可以了。

**部分 C — 领域文档。**

> 解释器：某些技能（`improve-codebase-architecture`、`diagnose`、`tdd`）读取 `CONTEXT.md` 文件来学习项目的领域语言，以及 `docs/adr/` 用于过去的架构决策。它们需要知道仓库是有一个全局上下文还是多个（例如，具有独立前端/后端上下文的 monorepo），以便在正确的地方查找。

确认布局：

- **单一上下文** — 一个 `CONTEXT.md` + `docs/adr/` 在仓库根目录。大多数仓库是这样。
- **多上下文** — `CONTEXT-MAP.md` 在根目录，指向每个上下文的 `CONTEXT.md`（通常是 monorepo）。

### 3. 确认和编辑

向用户展示草稿：

- 要添加到 `CLAUDE.md` / `AGENTS.md` 的 `## Agent skills` 块（见步骤 4 的选择规则）
- `docs/agents/issue-tracker.md`、`docs/agents/triage-labels.md`、`docs/agents/domain.md` 的内容

让他们在写入之前编辑。

### 4. 写入

**选择要编辑的文件：**

- 如果 `CLAUDE.md` 存在，编辑它。
- 否则如果 `AGENTS.md` 存在，编辑它。
- 如果都不存在，询问用户要创建哪一个——不要替他们选择。

当 `CLAUDE.md` 已存在时永远不要创建 `AGENTS.md`（反之亦然）——始终编辑已存在的那个。

如果在选定的文件中已存在 `## Agent skills` 块，就地更新其内容，而不是附加重复。不要覆盖用户对周围部分的编辑。

块：

```markdown
## Agent skills

### Issue tracker

[Issue 所在位置的一行摘要]。见 `docs/agents/issue-tracker.md`。

### Triage labels

[标签词汇表的一行摘要]。见 `docs/agents/triage-labels.md`。

### Domain docs

[布局的一行摘要——"单一上下文"或"多上下文"]。见 `docs/agents/domain.md`。
```

然后使用此技能文件夹中的种子模板作为起点编写三个 docs 文件：

- [issue-tracker-github.md](./issue-tracker-github.md) — GitHub Issue 追踪器
- [issue-tracker-gitlab.md](./issue-tracker-gitlab.md) — GitLab Issue 追踪器
- [issue-tracker-local.md](./issue-tracker-local.md) — 本地-markdown Issue 追踪器
- [triage-labels.md](./triage-labels.md) — 标签映射
- [domain.md](./domain.md) — 领域文档消费者规则 + 布局

对于"其他"Issue 追踪器，使用用户的描述从头开始编写 `docs/agents/issue-tracker.md`。

### 5. 完成

告诉用户设置完成，以及哪些工程技能现在会从这些文件读取。提及他们稍后可以直接编辑 `docs/agents/*.md`——只有在想要切换 Issue 追踪器或从头重启时才需要重新运行此技能。
