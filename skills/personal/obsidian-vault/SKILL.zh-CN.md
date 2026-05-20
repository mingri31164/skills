---
name: obsidian-vault
description: 在 Obsidian 知识库中搜索、创建和管理笔记，使用 wikilinks 和索引笔记。当用户想要在 Obsidian 中查找、创建或组织笔记时使用。
---

# Obsidian 知识库

## 知识库位置

`/mnt/d/Obsidian Vault/AI Research/`

大多数在根目录是扁平的。

## 命名约定

- **索引笔记**：聚合相关主题（例如 `Ralph Wiggum Index.md`、`Skills Index.md`、`RAG Index.md`）
- **所有笔记名称用标题大小写**
- 不用文件夹组织——使用链接和索引笔记代替

## 链接

- 使用 Obsidian `[[wikilinks]]` 语法：`[[Note Title]]`
- 笔记在底部链接到依赖项/相关笔记
- 索引笔记只是 `[[wikilinks]]` 列表

## 工作流

### 搜索笔记

```bash
# 按文件名搜索
find "/mnt/d/Obsidian Vault/AI Research/" -name "*.md" | grep -i "keyword"

# 按内容搜索
grep -rl "keyword" "/mnt/d/Obsidian Vault/AI Research/" --include="*.md"
```

或者直接在知识库路径上使用 Grep/Glob 工具。

### 创建新笔记

1. 使用**标题大小写**作为文件名
2. 按学习单元编写内容（按知识库规则）
3. 在底部添加 `[[wikilinks]]` 到相关笔记
4. 如果是编号序列的一部分，使用层次编号方案

### 查找相关笔记

在知识库中搜索 `[[Note Title]]` 以找到反向链接：

```bash
grep -rl "\\[\\[Note Title\\]\\]" "/mnt/d/Obsidian Vault/AI Research/"
```

### 查找索引笔记

```bash
find "/mnt/d/Obsidian Vault/AI Research/" -name "*Index*"
```
