# 何时模拟

仅在**系统边界**模拟：

- 外部 API（支付、邮件等）
- 数据库（有时——优先测试数据库）
- 时间/随机性
- 文件系统（有时）

不要模拟：

- 你自己的类/模块
- 内部协作者
- 你控制的任何东西

## 为可模拟性设计

在系统边界，设计易于模拟的接口：

**1. 使用依赖注入**

传入外部依赖而不是在内部创建它们：

```typescript
// 易于模拟
function processPayment(order, paymentClient) {
  return paymentClient.charge(order.total);
}

// 难以模拟
function processPayment(order) {
  const client = new StripeClient(process.env.STRIPE_KEY);
  return client.charge(order.total);
}
```

**2. 优先 SDK 风格接口而不是通用提取器**

为每个外部操作创建特定函数，而不是一个带条件逻辑的通用函数：

```typescript
// 好：每个函数独立可模拟
const api = {
  getUser: (id) => fetch(`/users/${id}`),
  getOrders: (userId) => fetch(`/users/${userId}/orders`),
  createOrder: (data) => fetch('/orders', { method: 'POST', body: data }),
};

// 坏：模拟需要条件逻辑
const api = {
  fetch: (endpoint, options) => fetch(endpoint, options),
};
```

SDK 方法意味着：
- 每个模拟返回一个特定形状
- 测试设置中没有条件逻辑
- 更易于看到测试练习哪些端点
- 每个端点有类型安全
