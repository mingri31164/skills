# HTML 报告格式

架构审查作为 OS 临时目录中的单个自包含 HTML 文件呈现。Tailwind 和 Mermaid 都来自 CDN。Mermaid 可靠地处理图形化图表；手工构建的 div 和内联 SVG 处理更多编辑性可视化（质量图、截面、折叠动画）。混合两者——不要在每件事上都依赖 Mermaid，它会开始看起来通用。

## 脚手架

```html
<!doctype html>
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <title>Architecture review — {{repo name}}</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script type="module">
      import mermaid from "https://cdn.jsdelivr.net/npm/mermaid@11/dist/mermaid.esm.min.mjs";
      mermaid.initialize({ startOnLoad: true, theme: "neutral", securityLevel: "loose" });
    </script>
    <style>
      /* Tailwind 覆盖不干净内容的小自定义层：
         虚线接缝线、手绘感觉的箭头等。 */
      .seam { stroke-dasharray: 4 4; }
      .leak { stroke: #dc2626; }
      .deep { background: linear-gradient(135deg, #0f172a, #1e293b); }
    </style>
  </head>
  <body class="bg-stone-50 text-slate-900 font-sans">
    <main class="max-w-5xl mx-auto px-6 py-12 space-y-12">
      <header>...</header>
      <section id="candidates" class="space-y-10">...</section>
      <section id="top-recommendation">...</section>
    </main>
  </body>
</html>
```

## 头部

仓库名称、日期和紧凑图例：实心框 = 模块，虚线 = 接缝，红色箭头 = 泄漏，粗深色框 = 深度模块。没有介绍段落——直接进入候选者。

## 候选卡片

图表承载重量。散文稀疏、朴素，使用词汇表术语（[LANGUAGE.md](LANGUAGE.md)）而不矫揉造作。

每个候选者是一个 `<article>`：

- **标题** — 简短，命名深化（例如"折叠订单接收管道"）。
- **徽章行** — 建议强度（`强烈` = 翠绿，`值得探索` = 琥珀，`推测性` = 板岩），加上依赖类别标签（`in-process`、`local-substitutable`、`ports & adapters`、`mock`）。
- **文件** — 等宽列表，`font-mono text-sm`。
- **之前 / 之后图表** — 中心件。并排两列。见下面的模式。
- **问题** — 一句话。什么伤害。
- **解决方案** — 一句话。什么改变。
- **收益** — 项目符号，每个 ≤6 个词。例如"测试命中一个接口"、"定价逻辑停止泄漏"、"删除 4 个浅包装器"。
- **ADR 标注**（如果适用）——琥珀色背景框中的一行。

没有解释段落。如果图表需要段落才能理解，重新绘制图表。

## 图表模式

选择适合候选者的模式。混合它们。不要让每个图表看起来一样——多样性是部分目的。

### Mermaid 图形（依赖/调用流的主力）

当重点是"X 调用 Y 调用 Z，看看这团乱麻"时使用 Mermaid `flowchart` 或 `graph`。将其包裹在 Tailwind 样式卡片中，这样不会感觉像是空降的。用 classDef 样式泄漏边缘为红色和深度模块为深色。序列图适合"之前：6 轮往返；之后：1。"。

```html
<div class="rounded-lg border border-slate-200 bg-white p-4">
  <pre class="mermaid">
    flowchart LR
      A[OrderHandler] --> B[OrderValidator]
      B --> C[OrderRepo]
      C -.leak.-> D[PricingClient]
      classDef leak stroke:#dc2626,stroke-width:2px;
      class C,D leak
  </pre>
</div>
```

### 手工构建的框和箭头（当 Mermaid 布局与你冲突时）

模块作为带边框和标签的 `<div>`。箭头作为内联 SVG `<line>` 或 `<path>` 元素，定位在相对容器上。当你想让"之后"图表感觉像一个带有灰色内部的一个粗边框深度模块时达到这个——Mermaid 不会用正确的权重渲染它。

### 截面（适合分层浅度）

堆叠水平带（`h-12 border-l-4`）来显示调用通过的层。之前：每个什么都不做的 6 个薄层。之后：1 个厚带，标有合并的责任。

### 质量图（适合"接口和实现一样宽"）

每个模块两个矩形——一个用于接口表面积，一个用于实现。之前：接口矩形几乎和实现矩形一样高（浅）。之后：接口矩形短，实现矩形高（深）。

### 调用图折叠

之前：作为嵌套框呈现的函数调用树。之后：同一树折叠成一个框，现在内部的调用在其中显示为暗淡。

## 样式指导

- 偏向编辑性，而不是企业仪表板。充裕留白。标题可选衬线（`font-serif` 与 stone/slate 配合良好）。
- 节制使用颜色：一个强调色（翠绿或靛蓝）加上红色表示泄漏和琥珀色表示警告。
- 保持图表约 320px 高，这样之前/之后并排舒适而不滚动。
- 在图表内对模块标签使用 `text-xs uppercase tracking-wider`——它们应该读起来像原理图，而不是 UI。
- 唯一脚本是 Tailwind CDN 和 Mermaid ESM 导入。否则报告是静态的——除了 Mermaid 自己的渲染，没有应用代码，没有交互性。

## 首要建议部分

一张更大的卡片。候选者名称，一句话说明原因，锚链接到其卡片。就这样。

## 语气

朴素英语，简洁——但架构名词和动词直接来自 [LANGUAGE.md](LANGUAGE.md)。简洁不是漂移的借口。

**精确使用：** module, interface, implementation, depth, deep, shallow, seam, adapter, leverage, locality。

**永远不要替代：** component, service, unit（用于 module）· API, signature（用于 interface）· boundary（用于 seam）· layer, wrapper（用于 module，当意思是 module 时）。

**适合风格的措辞：**

- "Order intake 模块是浅的——接口几乎匹配实现。"
- "定价泄漏跨接缝。"
- "深化：一个接口，一个测试地方。"
- "两个适配器证明接缝：HTTP 在 prod，内存在测试。"

**收益项目符号**用词汇表术语命名收益：_"局部性：bug 集中在一个模块"_, _"杠杆效应：一个接口，N 个调用点"_, _"接口缩小；实现吸收包装器"_。不要写_"更容易维护"_或_"更干净的代码"_——那些术语不在词汇表中，不能赢得它们的地位。

没有对冲，没有开场白，没有"值得注意的是……"。如果一句话可以是一个项目符号，让它成为项目符号。如果项目符号可以删除，删除它。如果一个术语不在 [LANGUAGE.md](LANGUAGE.md) 中，在发明新的之前先使用一个现有的。
