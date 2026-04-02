# @skillpet/circuit

[English](./README.md) | [简体中文](./README.zh-CN.md) | [日本語](./README.ja.md) | [한국어](./README.ko.md) | [Español](./README.es.md) | [Français](./README.fr.md) | [Deutsch](./README.de.md)

<p align="center">
  <strong>Librería de diagramas de circuitos — renderiza esquemas eléctricos desde JSON, con SVG interactivo, temas y componentes Vue / React.</strong>
</p>

<p align="center">
  <a href="https://www.npmjs.com/package/@skillpet/circuit"><img src="https://img.shields.io/npm/v/@skillpet/circuit.svg" alt="npm version"></a>
  <a href="https://www.npmjs.com/package/@skillpet/circuit"><img src="https://img.shields.io/npm/l/@skillpet/circuit.svg" alt="license"></a>
  <a href="https://circuit.skill.pet"><img src="https://img.shields.io/badge/docs-circuit.skill.pet-blue" alt="docs"></a>
</p>

---

**Sitio web & Demos:** [circuit.skill.pet](https://circuit.skill.pet)

## Características

- Más de 200 componentes eléctricos incorporados (resistencias, condensadores, diodos, transistores, ICs, puertas lógicas, etc.)
- Componentes Vue 3 y React listos para usar
- SVG interactivo: resaltado al pasar el ratón, tooltips, eventos de clic
- 3 temas incorporados (claro, oscuro, impresión) + temas personalizados
- Transiciones suaves de color entre componentes
- Renderizado de diagramas desde una descripción JSON simple
- Bundle para navegador (etiqueta script) & soporte ESM / CJS
- Renderizado de etiquetas con fórmulas KaTeX
- Diagramas de flujo, bloques DSP, componentes de protoboard
- Cero dependencias en tiempo de ejecución (KaTeX opcional)

## Instalación

```bash
npm install @skillpet/circuit
```

## Inicio Rápido

### React

```tsx
import { CircuitDiagram } from "@skillpet/circuit/react";

function App() {
  const circuit = {
    elements: [
      { type: "SourceV", d: "up", label: "12V" },
      { type: "ResistorIEEE", d: "right", label: "R1 10kΩ", id: "R1", tooltip: "Resistencia de carbón 100kΩ" },
      { type: "Capacitor", d: "down", label: "C1 100nF", id: "C1", tooltip: "Condensador cerámico 100nF" },
      { type: "Line", d: "left" },
      { type: "Ground" },
    ],
  };

  return (
    <CircuitDiagram
      circuit={circuit}
      interactive
      theme="light"
      onElementClick={(info) => console.log("Clic:", info.id)}
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
    { type: "ResistorIEEE", d: "right", label: "R1 10kΩ", id: "R1", tooltip: "Resistencia de carbón 100kΩ" },
    { type: "Capacitor", d: "down", label: "C1 100nF", id: "C1", tooltip: "Condensador cerámico 100nF" },
    { type: "Line", d: "left" },
    { type: "Ground" },
  ],
};

const onElementClick = (info) => console.log("Clic:", info.id);
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

// Renderizado estático — devuelve una cadena SVG
const svg = renderFromJson({
  elements: [
    { type: "SourceV", d: "up", label: "12V" },
    { type: "ResistorIEEE", d: "right", label: "R1 10kΩ" },
    { type: "Capacitor", d: "down", label: "C1 100nF" },
    { type: "Line", d: "left" },
    { type: "Ground" },
  ],
});

// Modo interactivo — montar en el DOM con hover, tooltips, eventos de clic
const ctrl = mountFromJson(document.getElementById("container")!, {
  elements: [
    { type: "ResistorIEEE", id: "R1", tooltip: "Resistencia de carbón 100kΩ" },
    { type: "Capacitor", d: "down", id: "C1", tooltip: "Cerámico 0.1μF" },
  ],
}, { interactive: true });

ctrl.on("element:click", (info) => console.log("Clic:", info.id));
```

### Navegador (CDN)

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

## Ejemplos en Vivo

Este repositorio contiene ejemplos HTML ejecutables — simplemente ábrelos en cualquier navegador:

| Archivo | Descripción |
|---------|-------------|
| [`index.html`](index.html) | Demo completa: circuitos básicos, temas, interactivo, transiciones de color |
| [`examples/01-basic.html`](examples/01-basic.html) | Circuito RC mínimo |
| [`examples/02-themes.html`](examples/02-themes.html) | Comparación de temas claro / oscuro / impresión |
| [`examples/03-interactive.html`](examples/03-interactive.html) | Modo interactivo con registro de eventos |
| [`examples/04-color-transitions.html`](examples/04-color-transitions.html) | Gradientes de color suaves entre componentes |

Todos los ejemplos cargan la librería desde el CDN [unpkg](https://unpkg.com/@skillpet/circuit/) — sin necesidad de compilación.

## Temas

Tres temas incorporados: `light` (predeterminado), `dark` y `print`.

```ts
const svg = renderFromJson(circuit, { theme: "dark" });
```

## Transiciones de Color

Transiciones suaves en gradiente entre componentes de diferentes colores:

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

## Licencia

El código de ejemplo en este repositorio tiene licencia MIT.

La librería `@skillpet/circuit` es gratuita para uso personal y educativo. El uso comercial requiere una licencia separada.
Para más detalles, visite [circuit.skill.pet/license](https://circuit.skill.pet/license) o contacte a **license@skill.pet**.
