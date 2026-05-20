---
name: setup-pre-commit
description: 在当前仓库设置 Husky pre-commit 钩子，包含 lint-staged（Prettier）、类型检查和测试。当用户想要添加 pre-commit 钩子、设置 Husky、配置 lint-staged，或添加提交时格式化/类型检查/测试时使用。
---

# 设置 Pre-Commit 钩子

## 这会设置什么

- **Husky** pre-commit 钩子
- **lint-staged** 在所有暂存文件上运行 Prettier
- **Prettier** 配置（如果缺失）
- **typecheck** 和 **test** 脚本在 pre-commit 钩子中

## 步骤

### 1. 检测包管理器

检查 `package-lock.json`（npm）、`pnpm-lock.yaml`（pnpm）、`yarn.lock`（yarn）、`bun.lockb`（bun）。使用存在的任何一个。如果不清楚默认为 npm。

### 2. 安装依赖

作为 devDependencies 安装：

```
husky lint-staged prettier
```

### 3. 初始化 Husky

```bash
npx husky init
```

这会创建 `.husky/` 目录并添加 `prepare: "husky"` 到 package.json。

### 4. 创建 `.husky/pre-commit`

写入此文件（Husky v9+ 不需要 shebang）：

```
npx lint-staged
npm run typecheck
npm run test
```

**适配**：将 `npm` 替换为检测到的包管理器。如果仓库的 package.json 中没有 `typecheck` 或 `test` 脚本，省略这些行并告知用户。

### 5. 创建 `.lintstagedrc`

```json
{
  "*": "prettier --ignore-unknown --write"
}
```

### 6. 创建 `.prettierrc`（如果缺失）

仅在没有 Prettier 配置时创建。使用这些默认值：

```json
{
  "useTabs": false,
  "tabWidth": 2,
  "printWidth": 80,
  "singleQuote": false,
  "trailingComma": "es5",
  "semi": true,
  "arrowParans": "always"
}
```

### 7. 验证

- [ ] `.husky/pre-commit` 存在且可执行
- [ ] `.lintstagedrc` 存在
- [ ] package.json 中的 `prepare` 脚本是 `"husky"`
- [ ] prettier 配置存在
- [ ] 运行 `npx lint-staged` 验证其工作

### 8. 提交

暂存所有更改/创建的文件并提交，消息为：`Add pre-commit hooks (husky + lint-staged + prettier)`

这会通过新的 pre-commit 钩子运行——一个好的冒烟测试，证明一切正常。

## 注意事项

- Husky v9+ 不需要在钩子文件中加 shebang
- `prettier --ignore-unknown` 跳过 Prettier 无法解析的文件（图片等）
- pre-commit 先运行 lint-staged（快速，仅暂存），然后运行完整 typecheck 和 tests
