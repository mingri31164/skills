# CONTEXT.md 格式

## 结构

```md
# {上下文名称}

{一两句话描述此上下文是什么以及为什么存在。}

## 语言

**Order**：
{术语一两句话描述}
_避免_：Purchase, transaction

**Invoice**：
交付后发送给客户的付款请求。
_避免_：Bill, payment request

**Customer**：
下订单的人或组织。
_避免_：Client, buyer, account
```

## 规则

- **要固执己见。** 当同一概念存在多个词时，选择最好的一个，并列出其他作为要避免的别名。
- **明确标记冲突。** 如果术语被模糊使用，在"标记的歧义"中用清晰的分辨率称呼它。
- **保持定义紧密。** 最多一两句话。定义它_是什么_，而不是它_做什么_。
- **展示关系。** 使用粗体术语名称并在明显的地方表达基数。
- **只包含特定于此项目上下文的术语。** 通用编程概念（超时、错误类型、实用模式）不属于，即使项目广泛使用它们。在添加术语之前问：这是此上下文独有的概念，还是通用编程概念？只有前者属于。
- **当自然集群出现时将术语分组在子标题下。** 如果所有术语属于一个内聚领域，扁平列表就好。
- **写示例对话。** 开发人员和领域专家之间的对话，展示术语如何自然交互并澄清相关概念之间的边界。

## 单一上下文 vs 多上下文仓库

**单一上下文（大多数仓库）：** 仓库根目录一个 `CONTEXT.md`。

**多个上下文：** 仓库根目录一个 `CONTEXT-MAP.md` 列出上下文、它们的位置以及它们如何相互关联：

```md
# 上下文地图

## 上下文

- [Ordering](./src/ordering/CONTEXT.md) — 接收和跟踪客户订单
- [Billing](./src/billing/CONTEXT.md) — 生成发票和处理付款
- [Fulfillment](./src/fulfillment/CONTEXT.md) — 管理仓库拣货和发货

## 关系

- **Ordering → Fulfillment**：Ordering 发出 `OrderPlaced` 事件；Fulfillment 消费它们开始拣货
- **Fulfillment → Billing**：Fulfillment 发出 `ShipmentDispatched` 事件；Billing 消费它们生成发票
- **Ordering ↔ Billing**：`CustomerId` 和 `Money` 的共享类型
```

技能推断适用哪种结构：

- 如果 `CONTEXT-MAP.md` 存在，阅读它以找到上下文
- 如果只有根 `CONTEXT.md` 存在，单一上下文
- 如果两者都不存在，在第一个术语被解决时惰性创建根 `CONTEXT.md`

当存在多个上下文时，推断当前主题涉及哪个。如果不清楚，问。
