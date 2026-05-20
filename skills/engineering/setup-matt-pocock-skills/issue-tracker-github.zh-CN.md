# Issue 追踪器：GitHub

此仓库的 Issue 和 PRD 作为 GitHub Issue 存在。使用 `gh` CLI 进行所有操作。

## 约定

- **创建 Issue**：`gh issue create --title "..." --body "..."`。对多行正文使用 heredoc。
- **读取 Issue**：`gh issue view <number> --comments`，用 `jq` 过滤评论，也获取标签。
- **列出 Issue**：`gh issue list --state open --json number,title,body,labels,comments --jq '[.[] | {number, title, body, labels: [.labels[].name], comments: [.comments[].body]}]'`，配合适当的 `--label` 和 `--state` 过滤器。
- **评论 Issue**：`gh issue comment <number> --body "..."`
- **应用/移除标签**：`gh issue edit <number> --add-label "..."` / `--remove-label "..."`
- **关闭**：`gh issue close <number> --comment "..."`

从 `git remote -v` 推断仓库——当在克隆内运行时 `gh` 自动这样做。

## 当技能说"发布到 Issue 追踪器"时

创建 GitHub Issue。

## 当技能说"获取相关工单"时

运行 `gh issue view <number> --comments`。
