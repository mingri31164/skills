# Issue 追踪器：本地 Markdown

此仓库的 Issue 和 PRD 作为 `.scratch/` 中的 markdown 文件存在。

## 约定

- 每个功能一个目录：`.scratch/<feature-slug>/`
- PRD 是 `.scratch/<feature-slug>/PRD.md`
- 实施 Issue 是 `.scratch/<feature-slug>/issues/<NN>-<slug>.md`，从 `01` 编号
- 分类状态记录为每个 Issue 文件顶部附近的 `Status:` 行（见 `triage-labels.md` 了解角色字符串）
- 评论和对话历史在 `## Comments` 标题下追加到文件底部

## 当技能说"发布到 Issue 追踪器"时

在 `.scratch/<feature-slug>/` 下创建新文件（必要时创建目录）。

## 当技能说"获取相关工单"时

读取引用路径处的文件。用户通常直接传递路径或 Issue 编号。
