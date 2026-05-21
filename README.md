<p>
  <a href="https://www.aihero.dev/s/skills-newsletter">
    <picture>
      <source media="(prefers-color-scheme: dark)" srcset="https://res.cloudinary.com/total-typescript/image/upload/v1777382277/skills-repo-dark_2x.png">
      <source media="(prefers-color-scheme: light)" srcset="https://res.cloudinary.com/total-typescript/image/upload/v1777382277/skill-repo-light_2x.png">
      <img alt="Skills" src="https://res.cloudinary.com/total-typescript/image/upload/v1777382277/skill-repo-light_2x.png" width="369">
    </picture>
  </a>
</p>

# 工程师必备技能

[English](./README.md) | [中文](./README.zh-CN.md)

[![skills.sh](https://skills.sh/b/mattpocock/skills)](https://skills.sh/mattpocock/skills)

我每天都在使用的 Agent 技能，用于做真正的工程工作——而不是"感觉编程"。

开发真正的应用程序很难。GSD、BMAD、Spec-Kit 等方法试图通过掌控流程来提供帮助。但在这样做的同时，它们剥夺了你的控制权，并使过程中的 bug 难以解决。

这些技能被设计成小巧、易于适配和可组合的。它们适用于任何模型，基于数十年的工程经验。你可以随意改造它们，让它们成为你自己的。享受这个过程。

如果你想了解这些技能的更新以及我创建的新技能，可以加入 ~60,000 名其他开发者订阅的Newsletter：

[订阅Newsletter](https://www.aihero.dev/s/skills-newsletter)

## 快速开始（30秒配置）

1. 运行 skills.sh 安装程序：

```bash
npx skills@latest add mattpocock/skills
```

2. 选择你想要的技能，以及你想将它们安装到哪些编码 Agent 上。**请务必选择 `/setup-matt-pocock-skills`**。

3. 在 Agent 中运行 `/setup-matt-pocock-skills`。它会：
   - 询问你想使用哪种 Issue 追踪器（GitHub、Linear 或本地文件）
   - 询问你在分类工单时使用什么标签（`/triage` 使用标签）
   - 询问你想在哪里保存我们创建的任何文档

4. 完成了——你可以开始使用了。

## 为什么存在这些技能

我构建这些技能是为了解决我在 Claude Code、Codex 和其他编码 Agent 中看到的常见失败模式。

### #1：Agent 没有做我想要的事

> "没有人能完全确定自己到底想要什么"
>
> David Thomas & Andrew Hunt, [程序员修炼之道](https://www.amazon.co.uk/Pragmatic-Programmer-Anniversary-Journey-Mastery/dp/B0833F1T3V)

**问题所在**。软件开发中最常见的失败模式是需求偏差。你认为开发者知道你的需求。然后你看到他们构建的东西——你才发现它完全误解了你。

在 AI 时代也是如此。你和 Agent 之间存在沟通鸿沟。解决方法是进行**深入质询**——让 Agent 问你关于你要构建内容的详细问题。

**解决方案**是使用：

- [`/grill-me`](./skills/productivity/grill-me/SKILL.zh-CN.md) —— 用于非代码场景
- [`/grill-with-docs`](./skills/engineering/grill-with-docs/SKILL.zh-CN.md) —— 与 [`/grill-me`](./skills/productivity/grill-me/SKILL.zh-CN.md) 相同，但增加了更多功能（见下文）

这些是我最受欢迎的技能。它们帮助你在开始之前与 Agent 达成一致，并深入思考你要做的改动。每次你想做改动时都使用它们。

### #2：Agent 输出过于冗长

> "有了通用语言，开发人员之间的对话和代码表达式都源自相同的领域模型。"
>
> Eric Evans, [领域驱动设计](https://www.amazon.co.uk/Domain-Driven-Design-Tackling-Complexity-Software/dp/0321125215)

**问题所在**：在一个项目的开始阶段，开发人员和他们正在为之构建软件的人（领域专家）通常说着不同的语言。

我在与 Agent 协作时也感受到了同样的紧张。Agent 通常被直接丢进一个项目中，被要求自己去弄清楚术语。所以它们用 20 个词来表达 1 个词就能说清楚的事。

**解决方案**是建立共享语言。这是一份帮助 Agent 解码项目中使用的术语的文档。

<details>
<summary>
示例
</summary>

这里有一个来自我的 `course-video-manager` 仓库的示例 [`CONTEXT.md`](https://github.com/mattpocock/course-video-manager/blob/076a5a7a182db0fe1e62971dd7a68bcadf010f1c/CONTEXT.md)。哪个更容易阅读？

- **之前**："当课程中某个章节内的课时被标记为'已实现'（即在文件系统中获得一个位置）时会出现问题"
- **之后**："物化级联存在问题"

这种简洁性在每个会话中都能带来回报。

</details>

这内置于 [`/grill-with-docs`](./skills/engineering/grill-with-docs/SKILL.zh-CN.md) 中。这是一次深入质询，但同时帮助你与 AI 建立共享语言，并在 ADR 中记录难以解释的决策。

这很难解释它有多强大。它可能是这个仓库中最酷的技巧。试试看，你会惊讶的。

> [!TIP]
> 共享语言除了减少冗长之外还有很多其他好处：
>
> - **变量、函数和文件的命名保持一致**，使用共享语言
> - 因此，**代码库更容易被 Agent 导航**
> - Agent 也会**消耗更少的 tokens 来思考**，因为它可以使用更简洁的语言

### #3：代码不工作

> "始终采取小的、审慎的步骤。反馈速度就是你的速度限制。永远不要接受一个太大的任务。"
>
> David Thomas & Andrew Hunt, [程序员修炼之道](https://www.amazon.co.uk/Pragmatic-Programmer-Anniversary-Journey-Mastery/dp/B0833F1T3V)

**问题所在**：假设你和 Agent 已经对齐了要构建什么。但如果 Agent _仍然_ 产出垃圾代码怎么办？

是时候审视你的反馈循环了。如果 Agent 无法获得其代码实际如何运行的反馈，它就会盲目飞行。

**解决方案**：你需要一套常规的反馈循环：静态类型、浏览器访问和自动化测试。

对于自动化测试，红色-绿色-重构循环至关重要。这就是 Agent 先写一个失败的测试，然后修复测试。这帮助 Agent 获得一致的反馈水平，从而产生更好的代码。

我构建了一个 **[`/tdd`](./skills/engineering/tdd/SKILL.zh-CN.md) 技能**，可以插入任何项目。它鼓励红色-绿色-重构，并为 Agent 提供大量关于什么是好测试和坏测试的指导。

对于调试，我还构建了 **[`/diagnose`](./skills/engineering/diagnose/SKILL.zh-CN.md)** 技能，将最佳调试实践封装成一个简单的循环。

### #4：我们构建了一团乱麻

> "每天都要投资于系统的设计。"
>
> Kent Beck, [极限编程解析](https://www.amazon.co.uk/Extreme-Programming-Explained-Embrace-Change/dp/0321278658)

> "最好的模块是深度的。它们允许通过简单的接口访问大量功能。"
>
> John Ousterhout, [软件设计哲学](https://www.amazon.co.uk/Philosophy-Software-Design-2nd/dp/173210221X)

**问题所在**：大多数用 Agent 构建的应用都很复杂且难以更改。因为 Agent 可以极大地加速编码，它们也会加速软件熵增。代码库以前所未有的速度变得更加复杂。

**解决方案**是一种全新的 AI 驱动开发方式：关心代码的设计。

这内置于这些技能的每一层：

- [`/to-prd`](./skills/engineering/to-prd/SKILL.zh-CN.md) 在创建 PRD 之前测试你正在修改哪些模块
- [`/zoom-out`](./skills/engineering/zoom-out/SKILL.zh-CN.md) 让 Agent 解释代码在整个系统中的上下文

最关键的是，[`/improve-codebase-architecture`](./skills/engineering/improve-codebase-architecture/SKILL.zh-CN.md) 帮助你拯救一个已经变成一团乱麻的代码库。我建议每隔几天在你的代码库上运行一次。

### 总结

软件工程基础比以往任何时候都更重要。这些技能是我将这些基础浓缩为可重复实践的最佳尝试，帮助你交付职业生涯中最好的应用。享受这个过程。

## 参考

### 工程技能

我日常用于代码工作的技能。

- **[diagnose](./skills/engineering/diagnose/SKILL.zh-CN.md)** — 硬性 bug 和性能回归的规范化诊断循环：复现 → 最小化 → 假设 → 插桩 → 修复 → 回归测试。
- **[grill-with-docs](./skills/engineering/grill-with-docs/SKILL.zh-CN.md)** — 深入质询会话，对抗现有领域模型的计划，优化术语，并在行内更新 `CONTEXT.md` 和 ADR。
- **[triage](./skills/engineering/triage/SKILL.zh-CN.md)** — 通过分类角色状态机对 Issue 进行分类。
- **[improve-codebase-architecture](./skills/engineering/improve-codebase-architecture/SKILL.zh-CN.md)** — 在代码库中找到深化机会，基于 `CONTEXT.md` 中的领域语言和 `docs/adr/` 中的决策。
- **[setup-matt-pocock-skills](./skills/engineering/setup-matt-pocock-skills/SKILL.zh-CN.md)** — 为每个仓库脚手架配置（Issue 追踪器、分类标签词汇表、领域文档布局），其他工程技能都会使用。 在使用 `to-issues`、`to-prd`、`triage`、`diagnose`、`tdd`、`improve-codebase-architecture` 或 `zoom-out` 之前，每个仓库运行一次。
- **[tdd](./skills/engineering/tdd/SKILL.zh-CN.md)** — 红色-绿色-重构循环的测试驱动开发。每次一个垂直切片来构建功能或修复 bug。
- **[to-issues](./skills/engineering/to-issues/SKILL.zh-CN.md)** — 使用垂直切片将任何计划、规范或 PRD 分解为可独立获取的 GitHub Issue。
- **[to-prd](./skills/engineering/to-prd/SKILL.zh-CN.md)** — 将当前对话上下文转化为 PRD 并作为 GitHub Issue 提交。不需要访谈——只是综合你已经在讨论的内容。
- **[zoom-out](./skills/engineering/zoom-out/SKILL.zh-CN.md)** — 让 Agent 缩小视野，在不熟悉的代码部分提供更广泛的上下文或更高层次的视角。
- **[prototype](./skills/engineering/prototype/SKILL.zh-CN.md)** — 构建一个可丢弃的原型来充实设计——对于状态/业务逻辑问题，可以是一个可运行的终端应用；对于 UI 问题，可以是几个可从一条路由切换的截然不同的 UI 变体。

### 生产力工具

通用的日常工作流工具，不限于代码。

- **[caveman](./skills/productivity/caveman/SKILL.zh-CN.md)** — 超级压缩的沟通模式。通过删除填充词同时保持完整的技术准确性，将 token 使用量减少约 75%。
- **[grill-me](./skills/productivity/grill-me/SKILL.zh-CN.md)** — 反复深入访谈你的计划或设计，直到决策树的每个分支都被解决。
- **[handoff](./skills/productivity/handoff/SKILL.zh-CN.md)** — 将当前对话压缩成一份交接文档，以便另一个 Agent 可以继续工作。
- **[write-a-skill](./skills/productivity/write-a-skill/SKILL.zh-CN.md)** — 创建具有正确结构、渐进式披露和捆绑资源的新技能。

### 杂项

我保留但不常用的工具。

- **[git-guardrails-claude-code](./skills/misc/git-guardrails-claude-code/SKILL.zh-CN.md)** — 设置 Claude Code 钩子，在危险 git 命令（push、reset --hard、clean 等）执行前进行阻止。
- **[migrate-to-shoehorn](./skills/misc/migrate-to-shoehorn/SKILL.zh-CN.md)** — 将测试文件从 `as` 类型断言迁移到 @total-typescript/shoehorn。
- **[scaffold-exercises](./skills/misc/scaffold-exercises/SKILL.zh-CN.md)** — 创建练习目录结构，包含章节、问题、解决方案和解释器。
- **[setup-pre-commit](./skills/misc/setup-pre-commit/SKILL.zh-CN.md)** — 设置 Husky pre-commit 钩子，包含 lint-staged、Prettier、类型检查和测试。
