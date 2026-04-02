# @skillpet/circuit

[English](./README.md) | [简体中文](./README.zh-CN.md) | [日本語](./README.ja.md) | [한국어](./README.ko.md) | [Español](./README.es.md) | [Français](./README.fr.md) | [Deutsch](./README.de.md)

<p align="center">
  <strong>回路図ライブラリ — JSON から電気回路図をレンダリング。インタラクティブ SVG、テーマ、Vue / React コンポーネント対応。</strong>
</p>

<p align="center">
  <a href="https://www.npmjs.com/package/@skillpet/circuit"><img src="https://img.shields.io/npm/v/@skillpet/circuit.svg" alt="npm version"></a>
  <a href="https://www.npmjs.com/package/@skillpet/circuit"><img src="https://img.shields.io/npm/l/@skillpet/circuit.svg" alt="license"></a>
  <a href="https://circuit.skill.pet"><img src="https://img.shields.io/badge/docs-circuit.skill.pet-blue" alt="docs"></a>
</p>

---

**公式サイト & デモ：** [circuit.skill.pet](https://circuit.skill.pet)

## 特徴

- 200以上の内蔵電気部品（抵抗、コンデンサ、ダイオード、トランジスタ、IC、ロジックゲートなど）
- Vue 3 & React コンポーネントをすぐに利用可能
- インタラクティブ SVG：ホバーハイライト、ツールチップ、クリックイベント
- 3つの内蔵テーマ（ライト、ダーク、印刷）+ カスタムテーマ
- コンポーネント間のスムーズな色遷移
- シンプルな JSON 記述から回路図をレンダリング
- ブラウザバンドル（script タグ）& ESM / CJS サポート
- KaTeX 数式ラベルレンダリング
- フローチャート、DSP ブロック、ブレッドボード部品
- ランタイム依存関係ゼロ（KaTeX はオプション）

## インストール

```bash
npm install @skillpet/circuit
```

## クイックスタート

### React

```tsx
import { CircuitDiagram } from "@skillpet/circuit/react";

function App() {
  const circuit = {
    elements: [
      { type: "SourceV", d: "up", label: "12V" },
      { type: "ResistorIEEE", d: "right", label: "R1 10kΩ", id: "R1", tooltip: "100kΩ カーボン抵抗" },
      { type: "Capacitor", d: "down", label: "C1 100nF", id: "C1", tooltip: "セラミックコンデンサ 100nF" },
      { type: "Line", d: "left" },
      { type: "Ground" },
    ],
  };

  return (
    <CircuitDiagram
      circuit={circuit}
      interactive
      theme="light"
      onElementClick={(info) => console.log("クリック:", info.id)}
      onElementHover={(info) => console.log("ホバー:", info.tooltip)}
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
    { type: "ResistorIEEE", d: "right", label: "R1 10kΩ", id: "R1", tooltip: "100kΩ カーボン抵抗" },
    { type: "Capacitor", d: "down", label: "C1 100nF", id: "C1", tooltip: "セラミックコンデンサ 100nF" },
    { type: "Line", d: "left" },
    { type: "Ground" },
  ],
};

const onElementClick = (info) => console.log("クリック:", info.id);
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

// 静的レンダリング — SVG 文字列を返す
const svg = renderFromJson({
  elements: [
    { type: "SourceV", d: "up", label: "12V" },
    { type: "ResistorIEEE", d: "right", label: "R1 10kΩ" },
    { type: "Capacitor", d: "down", label: "C1 100nF" },
    { type: "Line", d: "left" },
    { type: "Ground" },
  ],
});

// インタラクティブモード — DOM にマウント、ホバー・ツールチップ・クリックイベント対応
const ctrl = mountFromJson(document.getElementById("container")!, {
  elements: [
    { type: "ResistorIEEE", id: "R1", tooltip: "100kΩ カーボン抵抗" },
    { type: "Capacitor", d: "down", id: "C1", tooltip: "0.1μF セラミック" },
  ],
}, { interactive: true });

ctrl.on("element:click", (info) => console.log("クリック:", info.id));
```

### ブラウザ（CDN）

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

## ライブサンプル

このリポジトリにはブラウザで直接実行できる HTML サンプルが含まれています：

| ファイル | 説明 |
|----------|------|
| [`index.html`](index.html) | フルデモ：基本回路、テーマ、インタラクティブ、色遷移 |
| [`examples/01-basic.html`](examples/01-basic.html) | 最小 RC 回路 |
| [`examples/02-themes.html`](examples/02-themes.html) | ライト / ダーク / 印刷テーマの比較 |
| [`examples/03-interactive.html`](examples/03-interactive.html) | インタラクティブモードとイベントログ |
| [`examples/04-color-transitions.html`](examples/04-color-transitions.html) | コンポーネント間のスムーズな色グラデーション |

すべてのサンプルは [unpkg](https://unpkg.com/@skillpet/circuit/) CDN からライブラリを読み込みます — ビルド不要。

## テーマ

3つの内蔵テーマ：`light`（デフォルト）、`dark`、`print`。

```ts
const svg = renderFromJson(circuit, { theme: "dark" });
```

## 色遷移

異なる色のコンポーネント間でスムーズなグラデーション遷移：

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

## ライセンス

このリポジトリのサンプルコードは MIT ライセンスです。

`@skillpet/circuit` ライブラリ本体は個人・教育目的で無料利用可能です。商用利用には別途ライセンスが必要です。
詳細は [circuit.skill.pet/license](https://circuit.skill.pet/license) をご覧いただくか、**license@skill.pet** までお問い合わせください。
