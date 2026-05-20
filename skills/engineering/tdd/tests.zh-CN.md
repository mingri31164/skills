# 好的和坏的测试

## 好的测试

**集成风格**：通过真实接口测试，而不是模拟内部部分。

```typescript
// 好：测试可观察行为
test("user can checkout with valid cart", async () => {
  const cart = createCart();
  cart.add(product);
  const result = await checkout(cart, paymentMethod);
  expect(result.status).toBe("confirmed");
});
```

特征：

- 测试用户/调用者关心的行为
- 只使用公共 API
- 在内部重构中存活
- 描述 WHAT，而不是 HOW
- 每个测试一个逻辑断言

## 坏的测试

**实现细节测试**：与内部结构耦合。

```typescript
// 坏：测试实现细节
test("checkout calls paymentService.process", async () => {
  const mockPayment = jest.mock(paymentService);
  await checkout(cart, payment);
  expect(mockPayment.process).toHaveBeenCalledWith(cart.total);
});
```

危险信号：

- 模拟内部协作者
- 测试私有方法
- 断言调用次数/顺序
- 重构时测试中断但行为没有改变
- 测试名称描述 HOW 而不是 WHAT
- 通过外部方式而不是接口验证
