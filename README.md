# SYNAPSE

**[Live Demo: Synapse Algorithm Visualizer](https://synapse-beta-ten.vercel.app/)**

**SYNAPSE** is a zero-dependency, interactive Algorithm and Data Structure Visualizer built with React. It uses JavaScript generator functions and SVG to animate and explain computer science concepts in real-time.

## Features

* **Interactive Animations**: Step-by-step visualizations for algorithms and data structures.
* **Live Execution Log**: Real-time text output explaining logical comparisons at each step.
* **Complexity References**: Built-in Big-O time and space complexity cheat sheets.
* **Execution Controls**: Play, Stop, Reset, and Animation Speed adjustments.
* **Self-Contained**: Pure CSS custom properties injected via JavaScript with no external dependencies.

## Tech Stack

* **Framework**: React (Functional Components, Hooks).
* **Execution**: JavaScript `async function*` (generators) to control algorithmic steps without blocking the main thread.
* **Rendering**: DOM/CSS for arrays; dynamic `<svg>` for Trees, Graphs, and Linked Lists.

---
