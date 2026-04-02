# @skillpet/circuit

[English](./README.md) | [简体中文](./README.zh-CN.md) | [日本語](./README.ja.md) | [한국어](./README.ko.md) | [Español](./README.es.md) | [Français](./README.fr.md) | [Deutsch](./README.de.md)

<p align="center">
  <strong>Bibliothèque de schémas de circuits — rendez des schémas électriques depuis du JSON, avec SVG interactif, thèmes et composants Vue / React.</strong>
</p>

<p align="center">
  <a href="https://www.npmjs.com/package/@skillpet/circuit"><img src="https://img.shields.io/npm/v/@skillpet/circuit.svg" alt="npm version"></a>
  <a href="https://www.npmjs.com/package/@skillpet/circuit"><img src="https://img.shields.io/npm/l/@skillpet/circuit.svg" alt="license"></a>
  <a href="https://circuit.skill.pet"><img src="https://img.shields.io/badge/docs-circuit.skill.pet-blue" alt="docs"></a>
</p>

---

**Site web & Démos :** [circuit.skill.pet](https://circuit.skill.pet)

<p align="center">
  <img src="./images/rlc-circuit.svg" alt="RLC Circuit" height="120">
  &nbsp;&nbsp;&nbsp;
  <img src="./images/opamp.svg" alt="Op-Amp" height="120">
  &nbsp;&nbsp;&nbsp;
  <img src="./images/logic-gates.svg" alt="Logic Gates" height="120">
</p>

## Fonctionnalités

- Plus de 200 composants électriques intégrés (résistances, condensateurs, diodes, transistors, CI, portes logiques, etc.)
- Composants Vue 3 et React prêts à l'emploi
- SVG interactif : surbrillance au survol, infobulles, événements de clic
- 3 thèmes intégrés (clair, sombre, impression) + thèmes personnalisés
- Transitions de couleur fluides entre composants
- Rendu de diagrammes depuis une description JSON simple
- Bundle navigateur (balise script) & support ESM / CJS
- Rendu d'étiquettes avec formules KaTeX
- Organigrammes, blocs DSP, composants breadboard
- Zéro dépendance runtime (KaTeX optionnel)

## Installation

```bash
npm install @skillpet/circuit
```

## Démarrage Rapide

### React

```tsx
import { CircuitDiagram } from "@skillpet/circuit/react";

function App() {
  const circuit = {
    elements: [
      { type: "SourceV", d: "up", label: "12V" },
      { type: "ResistorIEEE", d: "right", label: "R1 10kΩ", id: "R1", tooltip: "Résistance carbone 100kΩ" },
      { type: "Capacitor", d: "down", label: "C1 100nF", id: "C1", tooltip: "Condensateur céramique 100nF" },
      { type: "Line", d: "left" },
      { type: "Ground" },
    ],
  };

  return (
    <CircuitDiagram
      circuit={circuit}
      interactive
      theme="light"
      onElementClick={(info) => console.log("Clic :", info.id)}
      onElementHover={(info) => console.log("Survol :", info.tooltip)}
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
    { type: "ResistorIEEE", d: "right", label: "R1 10kΩ", id: "R1", tooltip: "Résistance carbone 100kΩ" },
    { type: "Capacitor", d: "down", label: "C1 100nF", id: "C1", tooltip: "Condensateur céramique 100nF" },
    { type: "Line", d: "left" },
    { type: "Ground" },
  ],
};

const onElementClick = (info) => console.log("Clic :", info.id);
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

// Rendu statique — retourne une chaîne SVG
const svg = renderFromJson({
  elements: [
    { type: "SourceV", d: "up", label: "12V" },
    { type: "ResistorIEEE", d: "right", label: "R1 10kΩ" },
    { type: "Capacitor", d: "down", label: "C1 100nF" },
    { type: "Line", d: "left" },
    { type: "Ground" },
  ],
});

// Mode interactif — monter dans le DOM avec survol, infobulles, événements de clic
const ctrl = mountFromJson(document.getElementById("container")!, {
  elements: [
    { type: "ResistorIEEE", id: "R1", tooltip: "Résistance carbone 100kΩ" },
    { type: "Capacitor", d: "down", id: "C1", tooltip: "Céramique 0.1μF" },
  ],
}, { interactive: true });

ctrl.on("element:click", (info) => console.log("Clic :", info.id));
```

### Navigateur (CDN)

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

## Exemples en Direct

Ce dépôt contient des exemples HTML exécutables — ouvrez-les dans n'importe quel navigateur :

| Fichier | Description |
|---------|-------------|
| [`index.html`](index.html) | Démo complète : circuits de base, thèmes, interactif, transitions de couleur |
| [`examples/01-basic.html`](examples/01-basic.html) | Circuit RC minimal |
| [`examples/02-themes.html`](examples/02-themes.html) | Comparaison des thèmes clair / sombre / impression |
| [`examples/03-interactive.html`](examples/03-interactive.html) | Mode interactif avec journal d'événements |
| [`examples/04-color-transitions.html`](examples/04-color-transitions.html) | Dégradés de couleur fluides entre composants |

Tous les exemples chargent la bibliothèque depuis le CDN [unpkg](https://unpkg.com/@skillpet/circuit/) — aucune étape de compilation requise.

## Thèmes

Trois thèmes intégrés : `light` (par défaut), `dark` et `print`.

<p align="center">
  <img src="./images/rlc-circuit.svg" alt="Light Theme" height="100">
  &nbsp;&nbsp;&nbsp;&nbsp;
  <img src="./images/dark-theme.svg" alt="Dark Theme" height="100">
</p>

```ts
const svg = renderFromJson(circuit, { theme: "dark" });
```

## Transitions de Couleur

Transitions en dégradé fluide entre composants de couleurs différentes :

<p align="center">
  <img src="./images/color-transitions.svg" alt="Color Transitions" height="120">
</p>

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

## Licence

Le code d'exemple dans ce dépôt est sous licence MIT.

La bibliothèque `@skillpet/circuit` est gratuite pour un usage personnel et éducatif. L'utilisation commerciale nécessite une licence séparée.
Pour plus de détails, visitez [circuit.skill.pet/license](https://circuit.skill.pet/license) ou contactez **license@skill.pet**.
