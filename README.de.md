# @skillpet/circuit

[English](./README.md) | [简体中文](./README.zh-CN.md) | [日本語](./README.ja.md) | [한국어](./README.ko.md) | [Español](./README.es.md) | [Français](./README.fr.md) | [Deutsch](./README.de.md)

<p align="center">
  <strong>Schaltplan-Bibliothek — elektrische Schaltpläne aus JSON rendern, mit interaktivem SVG, Themes und Vue / React Komponenten.</strong>
</p>

<p align="center">
  <a href="https://www.npmjs.com/package/@skillpet/circuit"><img src="https://img.shields.io/npm/v/@skillpet/circuit.svg" alt="npm version"></a>
  <a href="https://www.npmjs.com/package/@skillpet/circuit"><img src="https://img.shields.io/npm/l/@skillpet/circuit.svg" alt="license"></a>
  <a href="https://circuit.skill.pet"><img src="https://img.shields.io/badge/docs-circuit.skill.pet-blue" alt="docs"></a>
</p>

---

**Website & Demos:** [circuit.skill.pet](https://circuit.skill.pet)

## Funktionen

- Über 200 integrierte elektrische Bauteile (Widerstände, Kondensatoren, Dioden, Transistoren, ICs, Logikgatter usw.)
- Vue 3 & React Komponenten sofort einsatzbereit
- Interaktives SVG: Hover-Hervorhebung, Tooltips, Klick-Events
- 3 integrierte Themes (Hell, Dunkel, Druck) + benutzerdefinierte Themes
- Fließende Farbübergänge zwischen Bauteilen
- Schaltpläne aus einer einfachen JSON-Beschreibung rendern
- Browser-Bundle (Script-Tag) & ESM / CJS Unterstützung
- KaTeX-Formel-Label-Rendering
- Flussdiagramme, DSP-Blöcke, Breadboard-Bauteile
- Keine Laufzeit-Abhängigkeiten (KaTeX optional)

## Installation

```bash
npm install @skillpet/circuit
```

## Schnellstart

### React

```tsx
import { CircuitDiagram } from "@skillpet/circuit/react";

function App() {
  const circuit = {
    elements: [
      { type: "SourceV", d: "up", label: "12V" },
      { type: "ResistorIEEE", d: "right", label: "R1 10kΩ", id: "R1", tooltip: "100kΩ Kohleschichtwiderstand" },
      { type: "Capacitor", d: "down", label: "C1 100nF", id: "C1", tooltip: "Keramikkondensator 100nF" },
      { type: "Line", d: "left" },
      { type: "Ground" },
    ],
  };

  return (
    <CircuitDiagram
      circuit={circuit}
      interactive
      theme="light"
      onElementClick={(info) => console.log("Klick:", info.id)}
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
    { type: "ResistorIEEE", d: "right", label: "R1 10kΩ", id: "R1", tooltip: "100kΩ Kohleschichtwiderstand" },
    { type: "Capacitor", d: "down", label: "C1 100nF", id: "C1", tooltip: "Keramikkondensator 100nF" },
    { type: "Line", d: "left" },
    { type: "Ground" },
  ],
};

const onElementClick = (info) => console.log("Klick:", info.id);
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

// Statisches Rendering — gibt einen SVG-String zurück
const svg = renderFromJson({
  elements: [
    { type: "SourceV", d: "up", label: "12V" },
    { type: "ResistorIEEE", d: "right", label: "R1 10kΩ" },
    { type: "Capacitor", d: "down", label: "C1 100nF" },
    { type: "Line", d: "left" },
    { type: "Ground" },
  ],
});

// Interaktiver Modus — in DOM einbinden mit Hover, Tooltips, Klick-Events
const ctrl = mountFromJson(document.getElementById("container")!, {
  elements: [
    { type: "ResistorIEEE", id: "R1", tooltip: "100kΩ Kohleschichtwiderstand" },
    { type: "Capacitor", d: "down", id: "C1", tooltip: "0.1μF Keramik" },
  ],
}, { interactive: true });

ctrl.on("element:click", (info) => console.log("Klick:", info.id));
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

## Live-Beispiele

Dieses Repository enthält ausführbare HTML-Beispiele — einfach in einem beliebigen Browser öffnen:

| Datei | Beschreibung |
|-------|-------------|
| [`index.html`](index.html) | Vollständige Demo: Grundschaltungen, Themes, Interaktiv, Farbübergänge |
| [`examples/01-basic.html`](examples/01-basic.html) | Minimale RC-Schaltung |
| [`examples/02-themes.html`](examples/02-themes.html) | Vergleich Hell / Dunkel / Druck Themes |
| [`examples/03-interactive.html`](examples/03-interactive.html) | Interaktiver Modus mit Event-Protokoll |
| [`examples/04-color-transitions.html`](examples/04-color-transitions.html) | Fließende Farbverläufe zwischen Bauteilen |

Alle Beispiele laden die Bibliothek vom [unpkg](https://unpkg.com/@skillpet/circuit/) CDN — kein Build-Schritt erforderlich.

## Themes

Drei integrierte Themes: `light` (Standard), `dark` und `print`.

```ts
const svg = renderFromJson(circuit, { theme: "dark" });
```

## Farbübergänge

Fließende Gradientenübergänge zwischen unterschiedlich gefärbten Bauteilen:

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

## Lizenz

Der Beispielcode in diesem Repository steht unter der MIT-Lizenz.

Die `@skillpet/circuit` Bibliothek selbst ist für den persönlichen und Bildungseinsatz kostenlos. Für die kommerzielle Nutzung ist eine separate Lizenz erforderlich.
Weitere Informationen finden Sie unter [circuit.skill.pet/license](https://circuit.skill.pet/license) oder kontaktieren Sie **license@skill.pet**.
