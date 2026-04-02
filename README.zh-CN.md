# @skillpet/circuit

[English](./README.md) | [简体中文](./README.zh-CN.md) | [日本語](./README.ja.md) | [한국어](./README.ko.md) | [Español](./README.es.md) | [Français](./README.fr.md) | [Deutsch](./README.de.md)

<p align="center">
  <strong>电路图绘制库 — 通过 JSON 描述渲染电气原理图，支持交互式 SVG、主题切换，以及 Vue / React 组件。</strong>
</p>

<p align="center">
  <a href="https://www.npmjs.com/package/@skillpet/circuit"><img src="https://img.shields.io/npm/v/@skillpet/circuit.svg" alt="npm version"></a>
  <a href="https://www.npmjs.com/package/@skillpet/circuit"><img src="https://img.shields.io/npm/l/@skillpet/circuit.svg" alt="license"></a>
  <a href="https://circuit.skill.pet"><img src="https://img.shields.io/badge/docs-circuit.skill.pet-blue" alt="docs"></a>
</p>

---

**官网 & 演示：** [circuit.skill.pet](https://circuit.skill.pet)

## 特性

- 200+ 内置电气元件（电阻、电容、二极管、晶体管、IC、逻辑门等）
- 开箱即用的 Vue 3 和 React 组件
- 交互式 SVG：悬停高亮、工具提示、点击事件
- 3 种内置主题（浅色、深色、打印）+ 自定义主题
- 元件间平滑颜色过渡
- 通过简单的 JSON 描述渲染电路图
- 浏览器端（script 标签）& ESM / CJS 支持
- KaTeX 数学公式标签渲染
- 流程图、DSP 模块、面包板实物元件
- 零运行时依赖（KaTeX 可选）

## 安装

```bash
npm install @skillpet/circuit
```

## 快速开始

### React

```tsx
import { CircuitDiagram } from "@skillpet/circuit/react";

function App() {
  const circuit = {
    elements: [
      { type: "SourceV", d: "up", label: "12V" },
      { type: "ResistorIEEE", d: "right", label: "R1 10kΩ", id: "R1", tooltip: "100kΩ 碳膜电阻" },
      { type: "Capacitor", d: "down", label: "C1 100nF", id: "C1", tooltip: "陶瓷电容 100nF" },
      { type: "Line", d: "left" },
      { type: "Ground" },
    ],
  };

  return (
    <CircuitDiagram
      circuit={circuit}
      interactive
      theme="light"
      onElementClick={(info) => console.log("点击:", info.id)}
      onElementHover={(info) => console.log("悬停:", info.tooltip)}
    />
  );
}
```

### Vue 3

```vue
<script setup>
import { CircuitDiagram } from "@skillpet/circuit/vue";

const circuit = {
  elements: [
    { type: "SourceV", d: "up", label: "12V" },
    { type: "ResistorIEEE", d: "right", label: "R1 10kΩ", id: "R1", tooltip: "100kΩ 碳膜电阻" },
    { type: "Capacitor", d: "down", label: "C1 100nF", id: "C1", tooltip: "陶瓷电容 100nF" },
    { type: "Line", d: "left" },
    { type: "Ground" },
  ],
};

const onElementClick = (info) => console.log("点击:", info.id);
</script>

<template>
  <CircuitDiagram
    :circuit="circuit"
    interactive
    theme="light"
    @element-click="onElementClick"
  />
</template>
```

### ESM / TypeScript

```ts
import { renderFromJson, mountFromJson } from "@skillpet/circuit";

// 静态渲染 — 返回 SVG 字符串
const svg = renderFromJson({
  elements: [
    { type: "SourceV", d: "up", label: "12V" },
    { type: "ResistorIEEE", d: "right", label: "R1 10kΩ" },
    { type: "Capacitor", d: "down", label: "C1 100nF" },
    { type: "Line", d: "left" },
    { type: "Ground" },
  ],
});

// 交互模式 — 挂载到 DOM，支持悬停、提示、点击事件
const ctrl = mountFromJson(document.getElementById("container")!, {
  elements: [
    { type: "ResistorIEEE", id: "R1", tooltip: "100kΩ 碳膜电阻" },
    { type: "Capacitor", d: "down", id: "C1", tooltip: "0.1μF 陶瓷电容" },
  ],
}, { interactive: true });

ctrl.on("element:click", (info) => console.log("点击:", info.id));
```

### 浏览器（CDN）

```html
<div id="output"></div>
<script src="https://unpkg.com/@skillpet/circuit/dist/circuit.bundle.min.js"></script>
<script>
  document.getElementById("output").innerHTML = Circuit.renderFromJson({
    elements: [
      { type: "SourceV", d: "up", label: "12V" },
      { type: "ResistorIEEE", d: "right", label: "R1 10kΩ" },
      { type: "Capacitor", d: "down", label: "C1 100nF" },
      { type: "Line", d: "left" },
      { type: "Ground" },
    ],
  });
</script>
```

## 在线示例

本仓库包含可直接运行的 HTML 示例 — 在浏览器中打开即可：

| 文件 | 说明 |
|------|------|
| [`index.html`](index.html) | 完整演示页：基础电路、主题切换、交互模式、颜色过渡 |
| [`examples/01-basic.html`](examples/01-basic.html) | 最小 RC 电路 |
| [`examples/02-themes.html`](examples/02-themes.html) | 浅色 / 深色 / 打印 主题对比 |
| [`examples/03-interactive.html`](examples/03-interactive.html) | 交互模式与事件日志 |
| [`examples/04-color-transitions.html`](examples/04-color-transitions.html) | 元件间平滑颜色渐变 |

所有示例通过 [unpkg](https://unpkg.com/@skillpet/circuit/) CDN 加载库 — 无需构建步骤。

## 主题

三种内置主题：`light`（默认）、`dark` 和 `print`。

```ts
const svg = renderFromJson(circuit, { theme: "dark" });
```

## 颜色过渡

不同颜色元件之间的平滑渐变过渡：

```ts
const svg = renderFromJson({
  drawing: { colorTransition: true },
  elements: [
    { type: "SourceV", d: "up", color: "#2ecc71" },
    { type: "ResistorIEEE", d: "right", color: "#e74c3c" },
    { type: "Capacitor", d: "down", color: "#3498db" },
    { type: "Line", d: "left" },
    { type: "Ground" },
  ],
}, { colorTransition: true });
```

## 许可证

本仓库中的示例代码使用 MIT 许可证。

`@skillpet/circuit` 库本身供个人和教育用途免费使用，商业使用需要单独授权。
详情请访问 [circuit.skill.pet/license](https://circuit.skill.pet/license) 或联系 **license@skill.pet**。
