# @skillpet/circuit

<p align="center">
  <strong>Circuit diagram library — render electrical schematics from JSON, with interactive SVG, themes, and Vue / React components.</strong>
</p>

<p align="center">
  <a href="https://www.npmjs.com/package/@skillpet/circuit"><img src="https://img.shields.io/npm/v/@skillpet/circuit.svg" alt="npm version"></a>
  <a href="https://www.npmjs.com/package/@skillpet/circuit"><img src="https://img.shields.io/npm/l/@skillpet/circuit.svg" alt="license"></a>
  <a href="https://circuit.skill.pet"><img src="https://img.shields.io/badge/docs-circuit.skill.pet-blue" alt="docs"></a>
</p>

---

**Website & Demos:** [circuit.skill.pet](https://circuit.skill.pet)

## Features

- 200+ built-in electrical components (resistors, capacitors, diodes, transistors, ICs, logic gates, etc.)
- Vue 3 & React components out of the box
- Interactive SVG: hover highlights, tooltips, click events
- 3 built-in themes (light, dark, print) + custom themes
- Smooth color transitions between elements
- Render circuit diagrams from a simple JSON description
- Browser bundle (script tag) & ESM / CJS support
- KaTeX math label rendering
- Flow charts, DSP blocks, pictorial breadboard components
- Zero runtime dependencies (except optional KaTeX)

## Installation

```bash
npm install @skillpet/circuit
```

## Quick Start

### React

```tsx
import { CircuitDiagram } from "@skillpet/circuit/react";

function App() {
  const circuit = {
    elements: [
      { type: "SourceV", d: "up", label: "12V" },
      { type: "ResistorIEEE", d: "right", label: "R1 10kΩ", id: "R1", tooltip: "100kΩ Carbon Film" },
      { type: "Capacitor", d: "down", label: "C1 100nF", id: "C1", tooltip: "Ceramic 100nF" },
      { type: "Line", d: "left" },
      { type: "Ground" },
    ],
  };

  return (
    <CircuitDiagram
      circuit={circuit}
      interactive
      theme="light"
      onElementClick={(info) => console.log("Clicked:", info.id)}
      onElementHover={(info) => console.log("Hover:", info.tooltip)}
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
    { type: "ResistorIEEE", d: "right", label: "R1 10kΩ", id: "R1", tooltip: "100kΩ Carbon Film" },
    { type: "Capacitor", d: "down", label: "C1 100nF", id: "C1", tooltip: "Ceramic 100nF" },
    { type: "Line", d: "left" },
    { type: "Ground" },
  ],
};

const onElementClick = (info) => console.log("Clicked:", info.id);
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

// Static rendering — returns an SVG string
const svg = renderFromJson({
  elements: [
    { type: "SourceV", d: "up", label: "12V" },
    { type: "ResistorIEEE", d: "right", label: "R1 10kΩ" },
    { type: "Capacitor", d: "down", label: "C1 100nF" },
    { type: "Line", d: "left" },
    { type: "Ground" },
  ],
});

// Interactive mode — mount into DOM with hover, tooltips, click events
const ctrl = mountFromJson(document.getElementById("container")!, {
  elements: [
    { type: "ResistorIEEE", id: "R1", tooltip: "100kΩ Carbon Film" },
    { type: "Capacitor", d: "down", id: "C1", tooltip: "0.1μF Ceramic" },
  ],
}, { interactive: true });

ctrl.on("element:click", (info) => console.log("Clicked:", info.id));
```

### Browser (CDN)

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

## Live Examples

This repo contains runnable HTML examples — just open them in any browser:

| File | Description |
|------|-------------|
| [`index.html`](index.html) | Full demo page: basic circuits, themes, interactive, color transitions |
| [`examples/01-basic.html`](examples/01-basic.html) | Minimal RC circuit |
| [`examples/02-themes.html`](examples/02-themes.html) | Light / Dark / Print theme comparison |
| [`examples/03-interactive.html`](examples/03-interactive.html) | Interactive mode with event logging |
| [`examples/04-color-transitions.html`](examples/04-color-transitions.html) | Smooth color gradient between elements |

All examples load the library from the [unpkg](https://unpkg.com/@skillpet/circuit/) CDN — no build step required.

## Themes

Three built-in themes: `light` (default), `dark`, and `print`.

```ts
const svg = renderFromJson(circuit, { theme: "dark" });
```

## Color Transitions

Smooth gradient transitions between differently colored elements:

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

## License

The example code in this repository is MIT licensed.

The `@skillpet/circuit` library itself is free for personal and educational use. Commercial use requires a separate license.
For details, see [circuit.skill.pet/license](https://circuit.skill.pet/license) or contact **license@skill.pet**.
