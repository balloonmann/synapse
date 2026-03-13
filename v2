/*
 * SYNAPSE v2 — Algorithm & Data Structure Visualizer
 * Architecture: Design Tokens → Primitives → Engines → Visualizers → Shell
 * Styling: CSS custom properties + <style> injection (no inline styles)
 * Grid: 8pt base unit throughout
 * Fonts: IBM Plex Mono (labels/code), IBM Plex Sans Condensed (headings)
 */

import { useState, useEffect, useRef, useCallback, useReducer } from "react";

// ─── 0. STYLE INJECTION ───────────────────────────────────────────────────────
const STYLES = `
@import url('https://fonts.googleapis.com/css2?family=IBM+Plex+Mono:wght@400;500;600&family=IBM+Plex+Sans+Condensed:wght@400;600;700&display=swap');

*, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

:root {
  /* Color tokens */
  --c-bg:         #0c0c0e;
  --c-surface:    #111114;
  --c-raised:     #18181d;
  --c-border:     #26262f;
  --c-border-hi:  #3a3a48;
  --c-text:       #e2e2e8;
  --c-text-2:     #8a8a9a;
  --c-text-3:     #52525e;
  --c-accent:     #f5a623;
  --c-accent-dim: #7a5112;
  --c-green:      #3dd68c;
  --c-green-dim:  #1a4d38;
  --c-red:        #f04a4a;
  --c-red-dim:    #4d1a1a;
  --c-blue:       #4a9eff;
  --c-blue-dim:   #1a3d6b;
  --c-purple:     #a78bfa;
  --c-purple-dim: #3b2d6b;

  /* Category accent colors */
  --cat-linear:   #4a9eff;
  --cat-hash:     #f5a623;
  --cat-tree:     #3dd68c;
  --cat-graph:    #a78bfa;
  --cat-search:   #f04a4a;
  --cat-sort:     #f5a623;
  --cat-advanced: #4a9eff;
  --cat-numeric:  #3dd68c;

  /* Spacing (8pt grid) */
  --sp-1: 4px;
  --sp-2: 8px;
  --sp-3: 12px;
  --sp-4: 16px;
  --sp-5: 20px;
  --sp-6: 24px;
  --sp-8: 32px;
  --sp-10: 40px;
  --sp-12: 48px;

  /* Typography */
  --font-mono: 'IBM Plex Mono', 'Courier New', monospace;
  --font-head: 'IBM Plex Sans Condensed', 'Arial Narrow', sans-serif;

  /* Radius */
  --r-sm: 4px;
  --r-md: 6px;
  --r-lg: 10px;

  /* Transitions */
  --t-fast: 120ms ease;
  --t-base: 200ms ease;
}

html, body, #root { height: 100%; background: var(--c-bg); }

/* ── Layout ── */
.synapse-shell {
  display: grid;
  grid-template-rows: 56px 1fr;
  grid-template-columns: 260px 1fr;
  height: 100vh;
  font-family: var(--font-mono);
  color: var(--c-text);
  background: var(--c-bg);
  overflow: hidden;
}

.synapse-header {
  grid-column: 1 / -1;
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 0 var(--sp-6);
  background: var(--c-surface);
  border-bottom: 1px solid var(--c-border);
  z-index: 10;
}

.synapse-sidebar {
  background: var(--c-surface);
  border-right: 1px solid var(--c-border);
  overflow-y: auto;
  overflow-x: hidden;
}

.synapse-main {
  overflow-y: auto;
  padding: var(--sp-6);
  background: var(--c-bg);
}

/* ── Header ── */
.header-brand {
  display: flex;
  align-items: baseline;
  gap: var(--sp-3);
}

.header-wordmark {
  font-family: var(--font-head);
  font-size: 20px;
  font-weight: 700;
  letter-spacing: 3px;
  color: var(--c-accent);
}

.header-tagline {
  font-size: 10px;
  letter-spacing: 2px;
  color: var(--c-text-3);
  text-transform: uppercase;
}

.header-meta {
  font-size: 11px;
  color: var(--c-text-3);
  letter-spacing: 1px;
}

/* ── Sidebar ── */
.sidebar-category {
  border-bottom: 1px solid var(--c-border);
}

.sidebar-cat-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  width: 100%;
  padding: var(--sp-3) var(--sp-4);
  background: none;
  border: none;
  cursor: pointer;
  color: var(--c-text-2);
  font-family: var(--font-mono);
  font-size: 10px;
  font-weight: 600;
  letter-spacing: 2px;
  text-transform: uppercase;
  text-align: left;
  transition: color var(--t-fast);
}

.sidebar-cat-header:hover { color: var(--c-text); }
.sidebar-cat-header:focus-visible {
  outline: 2px solid var(--c-accent);
  outline-offset: -2px;
}

.sidebar-cat-indicator {
  width: 6px;
  height: 6px;
  border-radius: 50%;
  flex-shrink: 0;
}

.sidebar-cat-chevron {
  font-size: 10px;
  transition: transform var(--t-base);
  color: var(--c-text-3);
}
.sidebar-cat-chevron.open { transform: rotate(90deg); }

.sidebar-items { overflow: hidden; }

.sidebar-item {
  display: block;
  width: 100%;
  padding: var(--sp-2) var(--sp-4) var(--sp-2) var(--sp-8);
  background: none;
  border: none;
  cursor: pointer;
  font-family: var(--font-mono);
  font-size: 12px;
  color: var(--c-text-3);
  text-align: left;
  transition: color var(--t-fast), background var(--t-fast);
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

.sidebar-item:hover { color: var(--c-text); background: var(--c-raised); }
.sidebar-item.active { color: var(--c-accent); background: var(--c-raised); }
.sidebar-item:focus-visible {
  outline: 2px solid var(--c-accent);
  outline-offset: -2px;
}

/* ── Primitive: Button ── */
.btn {
  display: inline-flex;
  align-items: center;
  gap: var(--sp-2);
  padding: var(--sp-2) var(--sp-4);
  border-radius: var(--r-md);
  font-family: var(--font-mono);
  font-size: 11px;
  font-weight: 500;
  letter-spacing: 1px;
  cursor: pointer;
  border: 1px solid transparent;
  transition: background var(--t-fast), border-color var(--t-fast), color var(--t-fast), opacity var(--t-fast);
  text-transform: uppercase;
  white-space: nowrap;
}
.btn:focus-visible { outline: 2px solid var(--c-accent); outline-offset: 2px; }
.btn:disabled { opacity: 0.4; cursor: not-allowed; }

.btn-primary {
  background: var(--c-accent);
  color: #000;
  border-color: var(--c-accent);
}
.btn-primary:hover:not(:disabled) { background: #d48f1a; }

.btn-secondary {
  background: transparent;
  color: var(--c-text-2);
  border-color: var(--c-border-hi);
}
.btn-secondary:hover:not(:disabled) { color: var(--c-text); border-color: var(--c-text-3); background: var(--c-raised); }

.btn-danger {
  background: var(--c-red-dim);
  color: var(--c-red);
  border-color: var(--c-red);
}
.btn-danger:hover:not(:disabled) { background: #6d2020; }

.btn-ghost {
  background: transparent;
  color: var(--c-text-3);
  border-color: transparent;
}
.btn-ghost:hover:not(:disabled) { color: var(--c-text); background: var(--c-raised); }

/* ── Primitive: Input ── */
.input {
  background: var(--c-raised);
  border: 1px solid var(--c-border);
  border-radius: var(--r-md);
  color: var(--c-text);
  font-family: var(--font-mono);
  font-size: 12px;
  padding: var(--sp-2) var(--sp-3);
  outline: none;
  transition: border-color var(--t-fast);
  width: 100%;
}
.input:focus { border-color: var(--c-accent); }
.input::placeholder { color: var(--c-text-3); }

/* ── Primitive: Range ── */
.range {
  -webkit-appearance: none;
  appearance: none;
  width: 100%;
  height: 3px;
  background: var(--c-border-hi);
  border-radius: 2px;
  outline: none;
  cursor: pointer;
}
.range::-webkit-slider-thumb {
  -webkit-appearance: none;
  width: 14px; height: 14px;
  border-radius: 50%;
  background: var(--c-accent);
  cursor: pointer;
}
.range:focus-visible { outline: 2px solid var(--c-accent); outline-offset: 2px; }

/* ── Primitive: Select ── */
.select {
  background: var(--c-raised);
  border: 1px solid var(--c-border);
  border-radius: var(--r-md);
  color: var(--c-text);
  font-family: var(--font-mono);
  font-size: 11px;
  padding: var(--sp-2) var(--sp-3);
  outline: none;
  cursor: pointer;
}
.select:focus-visible { outline: 2px solid var(--c-accent); }

/* ── Primitive: Badge ── */
.badge {
  display: inline-block;
  padding: 2px var(--sp-2);
  border-radius: var(--r-sm);
  font-size: 10px;
  font-weight: 600;
  letter-spacing: 1px;
  text-transform: uppercase;
}

/* ── Panel ── */
.panel {
  background: var(--c-surface);
  border: 1px solid var(--c-border);
  border-radius: var(--r-lg);
  padding: var(--sp-6);
}

.panel-header {
  display: flex;
  align-items: flex-start;
  justify-content: space-between;
  margin-bottom: var(--sp-6);
  gap: var(--sp-4);
}

.panel-title {
  font-family: var(--font-head);
  font-size: 18px;
  font-weight: 700;
  letter-spacing: 1px;
  color: var(--c-text);
}

.panel-subtitle {
  font-size: 11px;
  color: var(--c-text-3);
  margin-top: var(--sp-1);
  letter-spacing: 0.5px;
}

.panel-controls {
  display: flex;
  align-items: center;
  gap: var(--sp-2);
  flex-wrap: wrap;
}

/* ── Info bar ── */
.info-bar {
  display: flex;
  gap: var(--sp-6);
  flex-wrap: wrap;
  padding: var(--sp-3) var(--sp-4);
  background: var(--c-raised);
  border: 1px solid var(--c-border);
  border-radius: var(--r-md);
  margin-bottom: var(--sp-4);
}

.info-stat { display: flex; flex-direction: column; gap: 2px; }
.info-stat-label { font-size: 9px; letter-spacing: 2px; color: var(--c-text-3); text-transform: uppercase; }
.info-stat-value { font-size: 13px; font-weight: 600; color: var(--c-accent); }

/* ── Legend ── */
.legend {
  display: flex;
  gap: var(--sp-4);
  flex-wrap: wrap;
  margin-top: var(--sp-4);
}

.legend-item {
  display: flex;
  align-items: center;
  gap: var(--sp-2);
  font-size: 10px;
  color: var(--c-text-3);
  letter-spacing: 0.5px;
}

.legend-dot {
  width: 10px;
  height: 10px;
  border-radius: var(--r-sm);
  flex-shrink: 0;
}

/* ── Empty state ── */
.empty-state {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: var(--sp-4);
  padding: var(--sp-12) var(--sp-6);
  text-align: center;
}

.empty-state-icon {
  width: 48px;
  height: 48px;
  border: 1px solid var(--c-border);
  border-radius: var(--r-lg);
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 20px;
  color: var(--c-text-3);
}

.empty-state-title {
  font-family: var(--font-head);
  font-size: 16px;
  font-weight: 600;
  color: var(--c-text-2);
}

.empty-state-body {
  font-size: 12px;
  color: var(--c-text-3);
  max-width: 280px;
  line-height: 1.7;
}

/* ── Skeleton loader ── */
@keyframes shimmer {
  0% { background-position: -400px 0; }
  100% { background-position: 400px 0; }
}

.skeleton {
  background: linear-gradient(90deg, var(--c-raised) 25%, var(--c-border) 50%, var(--c-raised) 75%);
  background-size: 800px 100%;
  animation: shimmer 1.6s infinite;
  border-radius: var(--r-sm);
}

/* ── Sort bars ── */
.sort-canvas {
  display: flex;
  align-items: flex-end;
  gap: 2px;
  height: 200px;
  padding: var(--sp-2) 0;
  background: var(--c-raised);
  border: 1px solid var(--c-border);
  border-radius: var(--r-md);
  padding: var(--sp-3);
}

.sort-bar {
  flex: 1;
  border-radius: 2px 2px 0 0;
  transition: height 80ms ease, background-color 120ms ease;
  min-width: 2px;
}

.sort-bar.idle     { background: var(--c-border-hi); }
.sort-bar.active   { background: var(--c-blue); }
.sort-bar.comparing { background: var(--c-accent); box-shadow: 0 0 6px var(--c-accent-dim); }
.sort-bar.swapped  { background: var(--c-red); }
.sort-bar.pivot    { background: var(--c-purple); }
.sort-bar.sorted   { background: var(--c-green); }

/* ── Grid (pathfinding) ── */
.pf-grid {
  display: grid;
  gap: 1px;
  background: var(--c-border);
  border: 1px solid var(--c-border);
  border-radius: var(--r-md);
  overflow: hidden;
  user-select: none;
  cursor: crosshair;
}

.pf-cell {
  aspect-ratio: 1;
  transition: background 60ms ease;
}

.pf-cell.empty    { background: var(--c-raised); }
.pf-cell.wall     { background: var(--c-border-hi); }
.pf-cell.start    { background: var(--c-green); }
.pf-cell.end      { background: var(--c-red); }
.pf-cell.visited  { background: var(--c-blue-dim); }
.pf-cell.path     { background: var(--c-accent); }
.pf-cell.frontier { background: var(--c-purple-dim); }

/* ── SVG canvas ── */
.svg-canvas {
  width: 100%;
  background: var(--c-raised);
  border: 1px solid var(--c-border);
  border-radius: var(--r-md);
  display: block;
}

/* ── Tree nodes ── */
.tree-node-circle {
  transition: stroke 150ms ease, fill 150ms ease;
}

/* ── Array cells ── */
.array-row {
  display: flex;
  gap: 2px;
  align-items: flex-end;
}

.array-cell {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: var(--sp-1);
}

.array-cell-box {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 40px;
  height: 40px;
  border: 1px solid var(--c-border);
  border-radius: var(--r-sm);
  font-size: 12px;
  font-weight: 600;
  background: var(--c-raised);
  color: var(--c-text);
  transition: background 150ms ease, border-color 150ms ease, color 150ms ease;
}

.array-cell-box.idle       { background: var(--c-raised); border-color: var(--c-border); }
.array-cell-box.active     { background: var(--c-blue-dim); border-color: var(--c-blue); color: var(--c-blue); }
.array-cell-box.comparing  { background: var(--c-accent-dim); border-color: var(--c-accent); color: var(--c-accent); }
.array-cell-box.swapped    { background: var(--c-red-dim); border-color: var(--c-red); color: var(--c-red); }
.array-cell-box.found      { background: var(--c-green-dim); border-color: var(--c-green); color: var(--c-green); }
.array-cell-box.pivot      { background: var(--c-purple-dim); border-color: var(--c-purple); color: var(--c-purple); }
.array-cell-box.sorted     { background: var(--c-green-dim); border-color: var(--c-green); color: var(--c-green); }
.array-cell-box.pointer-a  { background: var(--c-blue-dim); border-color: var(--c-blue); color: var(--c-blue); }
.array-cell-box.pointer-b  { background: var(--c-purple-dim); border-color: var(--c-purple); color: var(--c-purple); }
.array-cell-box.window     { background: var(--c-accent-dim); border-color: var(--c-accent); color: var(--c-accent); }

.array-cell-idx {
  font-size: 9px;
  color: var(--c-text-3);
  letter-spacing: 0.5px;
}

.array-cell-label {
  font-size: 10px;
  color: var(--c-accent);
  font-weight: 600;
  height: 14px;
}

/* ── Hash table ── */
.hash-table {
  display: flex;
  flex-direction: column;
  gap: 2px;
}

.hash-slot {
  display: flex;
  align-items: center;
  gap: var(--sp-3);
  padding: var(--sp-2) var(--sp-3);
  background: var(--c-raised);
  border: 1px solid var(--c-border);
  border-radius: var(--r-sm);
  transition: border-color 150ms ease, background 150ms ease;
}

.hash-slot.occupied { border-color: var(--c-accent); background: var(--c-accent-dim); }
.hash-slot.probing  { border-color: var(--c-blue); background: var(--c-blue-dim); }
.hash-slot.collision { border-color: var(--c-red); background: var(--c-red-dim); }

.hash-slot-idx {
  font-size: 10px;
  color: var(--c-text-3);
  min-width: 24px;
  font-weight: 600;
}

.hash-slot-chain {
  display: flex;
  gap: var(--sp-2);
  flex-wrap: wrap;
}

.hash-node {
  padding: 2px var(--sp-2);
  background: var(--c-accent-dim);
  border: 1px solid var(--c-accent);
  border-radius: var(--r-sm);
  font-size: 11px;
  color: var(--c-accent);
  font-weight: 600;
}

/* ── Matrix ── */
.matrix-grid {
  display: inline-grid;
  gap: 2px;
}

.matrix-cell {
  width: 44px;
  height: 36px;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 12px;
  font-weight: 600;
  border: 1px solid var(--c-border);
  border-radius: var(--r-sm);
  background: var(--c-raised);
  color: var(--c-text);
  transition: background 120ms ease, border-color 120ms ease;
}

.matrix-cell.active   { background: var(--c-accent-dim); border-color: var(--c-accent); color: var(--c-accent); }
.matrix-cell.result   { background: var(--c-green-dim); border-color: var(--c-green); color: var(--c-green); }
.matrix-cell.row-hi   { background: var(--c-blue-dim); border-color: var(--c-blue); }
.matrix-cell.col-hi   { background: var(--c-purple-dim); border-color: var(--c-purple); }

/* ── DP table ── */
.dp-table { border-collapse: collapse; font-size: 12px; }
.dp-cell {
  width: 40px; height: 36px;
  text-align: center;
  border: 1px solid var(--c-border);
  background: var(--c-raised);
  color: var(--c-text);
  transition: background 100ms, color 100ms;
}
.dp-cell.active   { background: var(--c-accent-dim); color: var(--c-accent); border-color: var(--c-accent); }
.dp-cell.filled   { background: var(--c-blue-dim); color: var(--c-blue); }
.dp-cell.optimal  { background: var(--c-green-dim); color: var(--c-green); border-color: var(--c-green); }
.dp-cell.header   { background: var(--c-border); color: var(--c-text-2); font-weight: 600; font-size: 10px; }

/* ── Scrollbar ── */
.synapse-sidebar::-webkit-scrollbar { width: 4px; }
.synapse-sidebar::-webkit-scrollbar-track { background: transparent; }
.synapse-sidebar::-webkit-scrollbar-thumb { background: var(--c-border-hi); border-radius: 2px; }

/* ── Step log ── */
.step-log {
  height: 80px;
  overflow-y: auto;
  background: var(--c-raised);
  border: 1px solid var(--c-border);
  border-radius: var(--r-md);
  padding: var(--sp-2) var(--sp-3);
  margin-top: var(--sp-4);
  font-size: 11px;
  color: var(--c-text-2);
  line-height: 1.8;
}

.step-log-entry { color: var(--c-text-3); }
.step-log-entry.current { color: var(--c-accent); font-weight: 600; }

/* ── Trie ── */
.trie-word-list {
  display: flex;
  flex-wrap: wrap;
  gap: var(--sp-2);
  margin-top: var(--sp-3);
}

.trie-word-tag {
  padding: var(--sp-1) var(--sp-3);
  background: var(--c-blue-dim);
  border: 1px solid var(--c-blue);
  border-radius: var(--r-sm);
  font-size: 11px;
  color: var(--c-blue);
  font-weight: 600;
}

.trie-word-tag.highlighted {
  background: var(--c-accent-dim);
  border-color: var(--c-accent);
  color: var(--c-accent);
}

/* ── Responsive ── */
@media (max-width: 768px) {
  .synapse-shell {
    grid-template-columns: 1fr;
    grid-template-rows: 56px auto 1fr;
  }
  .synapse-sidebar { display: none; }
}
`;

// Inject styles once
if (!document.getElementById("synapse-styles")) {
  const el = document.createElement("style");
  el.id = "synapse-styles";
  el.textContent = STYLES;
  document.head.appendChild(el);
}

// ─── 1. CONSTANTS & CATALOG ───────────────────────────────────────────────────

const sleep = (ms) => new Promise((r) => setTimeout(r, ms));

const CATEGORIES = [
  {
    id: "linear",
    label: "Linear Structures",
    color: "var(--cat-linear)",
    items: [
      { id: "arrays",         label: "Arrays" },
      { id: "strings",        label: "Strings" },
      { id: "singly-ll",      label: "Singly Linked List" },
      { id: "doubly-ll",      label: "Doubly Linked List" },
      { id: "stacks",         label: "Stacks" },
      { id: "queues",         label: "Queues" },
      { id: "circular-queue", label: "Circular Queue" },
    ],
  },
  {
    id: "hash",
    label: "Hash Structures",
    color: "var(--cat-hash)",
    items: [
      { id: "hash-table",     label: "Hash Table" },
      { id: "hash-map",       label: "Hash Map" },
      { id: "collision",      label: "Collision Resolution" },
    ],
  },
  {
    id: "trees",
    label: "Trees",
    color: "var(--cat-tree)",
    items: [
      { id: "binary-tree",    label: "Binary Tree" },
      { id: "bst",            label: "Binary Search Tree" },
      { id: "avl",            label: "AVL Tree" },
      { id: "heap-struct",    label: "Heap" },
      { id: "priority-queue", label: "Priority Queue" },
      { id: "trie",           label: "Trie (Prefix Tree)" },
      { id: "btree",          label: "B-Tree" },
    ],
  },
  {
    id: "graphs",
    label: "Graphs",
    color: "var(--cat-graph)",
    items: [
      { id: "directed",       label: "Directed Graph" },
      { id: "undirected",     label: "Undirected Graph" },
      { id: "weighted",       label: "Weighted Graph" },
      { id: "union-find",     label: "Disjoint Sets (Union-Find)" },
    ],
  },
  {
    id: "search",
    label: "Search & Traversal",
    color: "var(--cat-search)",
    items: [
      { id: "linear-search",  label: "Linear Search" },
      { id: "binary-search",  label: "Binary Search" },
      { id: "floyd-cycle",    label: "Floyd's Tortoise & Hare" },
      { id: "dfs",            label: "Depth-First Search" },
      { id: "bfs",            label: "Breadth-First Search" },
      { id: "tree-traversal", label: "Tree Traversals" },
      { id: "postfix",        label: "Postfix / Prefix Eval" },
    ],
  },
  {
    id: "sort",
    label: "Sorting & Techniques",
    color: "var(--cat-sort)",
    items: [
      { id: "merge-sort",     label: "Merge Sort" },
      { id: "quick-sort",     label: "Quick Sort" },
      { id: "two-pointer",    label: "Two-Pointer Technique" },
      { id: "sliding-window", label: "Sliding Window" },
    ],
  },
  {
    id: "advanced",
    label: "Advanced Algorithms",
    color: "var(--cat-advanced)",
    items: [
      { id: "dijkstra",       label: "Dijkstra's Algorithm" },
      { id: "astar",          label: "A* Search" },
      { id: "kruskal",        label: "Kruskal's Algorithm" },
      { id: "prim",           label: "Prim's Algorithm" },
      { id: "topological",    label: "Topological Sort" },
      { id: "dp-fib",         label: "Dynamic Programming (Fib)" },
      { id: "dp-knapsack",    label: "DP: Knapsack" },
      { id: "memoization",    label: "Memoization" },
      { id: "tabulation",     label: "Tabulation" },
    ],
  },
  {
    id: "numeric",
    label: "Numeric & Matrix",
    color: "var(--cat-numeric)",
    items: [
      { id: "matrix-mult",    label: "Matrix Multiplication" },
      { id: "heapify",        label: "Heapify" },
      { id: "extract-minmax", label: "Extract Min/Max" },
      { id: "path-compress",  label: "Path Compression" },
      { id: "vectorized",     label: "Vectorized Operations" },
    ],
  },
];

const COMPLEXITY = {
  "arrays":         { time: "O(1) access", space: "O(n)" },
  "strings":        { time: "O(n) scan",   space: "O(n)" },
  "singly-ll":      { time: "O(n) search", space: "O(n)" },
  "doubly-ll":      { time: "O(n) search", space: "O(n)" },
  "stacks":         { time: "O(1) push/pop", space: "O(n)" },
  "queues":         { time: "O(1) enq/deq", space: "O(n)" },
  "circular-queue": { time: "O(1) enq/deq", space: "O(n)" },
  "hash-table":     { time: "O(1) avg",    space: "O(n)" },
  "hash-map":       { time: "O(1) avg",    space: "O(n)" },
  "collision":      { time: "O(1) avg",    space: "O(n)" },
  "binary-tree":    { time: "O(n)",        space: "O(n)" },
  "bst":            { time: "O(log n) avg", space: "O(n)" },
  "avl":            { time: "O(log n)",    space: "O(n)" },
  "heap-struct":    { time: "O(log n)",    space: "O(n)" },
  "priority-queue": { time: "O(log n)",    space: "O(n)" },
  "trie":           { time: "O(m) key len", space: "O(m*n)" },
  "btree":          { time: "O(log n)",    space: "O(n)" },
  "directed":       { time: "O(V+E)",      space: "O(V+E)" },
  "undirected":     { time: "O(V+E)",      space: "O(V+E)" },
  "weighted":       { time: "O(V+E)",      space: "O(V+E)" },
  "union-find":     { time: "O(alpha n)",  space: "O(n)" },
  "linear-search":  { time: "O(n)",        space: "O(1)" },
  "binary-search":  { time: "O(log n)",    space: "O(1)" },
  "floyd-cycle":    { time: "O(n)",        space: "O(1)" },
  "dfs":            { time: "O(V+E)",      space: "O(V)" },
  "bfs":            { time: "O(V+E)",      space: "O(V)" },
  "tree-traversal": { time: "O(n)",        space: "O(h)" },
  "postfix":        { time: "O(n)",        space: "O(n)" },
  "merge-sort":     { time: "O(n log n)",  space: "O(n)" },
  "quick-sort":     { time: "O(n log n) avg", space: "O(log n)" },
  "two-pointer":    { time: "O(n)",        space: "O(1)" },
  "sliding-window": { time: "O(n)",        space: "O(k)" },
  "dijkstra":       { time: "O((V+E) log V)", space: "O(V)" },
  "astar":          { time: "O(E log V)",  space: "O(V)" },
  "kruskal":        { time: "O(E log E)",  space: "O(V)" },
  "prim":           { time: "O(E log V)",  space: "O(V)" },
  "topological":    { time: "O(V+E)",      space: "O(V)" },
  "dp-fib":         { time: "O(n)",        space: "O(n)" },
  "dp-knapsack":    { time: "O(nW)",       space: "O(nW)" },
  "memoization":    { time: "O(n)",        space: "O(n)" },
  "tabulation":     { time: "O(n)",        space: "O(n)" },
  "matrix-mult":    { time: "O(n^3)",      space: "O(n^2)" },
  "heapify":        { time: "O(n)",        space: "O(1)" },
  "extract-minmax": { time: "O(log n)",    space: "O(1)" },
  "path-compress":  { time: "O(alpha n)",  space: "O(n)" },
  "vectorized":     { time: "O(n) parallel", space: "O(n)" },
};

// ─── 2. ALGORITHM ENGINES ─────────────────────────────────────────────────────

async function* bubbleSortGen(arr) {
  const a = [...arr], n = a.length;
  for (let i = 0; i < n - 1; i++) {
    for (let j = 0; j < n - i - 1; j++) {
      yield { arr: [...a], state: "comparing", indices: [j, j+1], sorted: Array.from({length:i}, (_,k) => n-1-k), log: `Compare a[${j}]=${a[j]} vs a[${j+1}]=${a[j+1]}` };
      if (a[j] > a[j+1]) {
        [a[j], a[j+1]] = [a[j+1], a[j]];
        yield { arr: [...a], state: "swapped", indices: [j, j+1], sorted: Array.from({length:i}, (_,k) => n-1-k), log: `Swap a[${j}] <-> a[${j+1}]` };
      }
    }
  }
  yield { arr: [...a], state: "done", sorted: Array.from({length:n},(_,k)=>k), log: "Sorted." };
}

async function* mergeSortGen(arr) {
  const a = [...arr];
  yield* _mergeSort(a, 0, a.length-1);
  yield { arr: [...a], state: "done", sorted: Array.from({length:a.length},(_,k)=>k), log: "Sorted." };
}
async function* _mergeSort(a, l, r) {
  if (l >= r) return;
  const m = Math.floor((l+r)/2);
  yield* _mergeSort(a, l, m);
  yield* _mergeSort(a, m+1, r);
  const left = a.slice(l, m+1), right = a.slice(m+1, r+1);
  let i=0, j=0, k=l;
  while (i < left.length && j < right.length) {
    yield { arr:[...a], state:"comparing", indices:[l+i, m+1+j], active:Array.from({length:r-l+1},(_,x)=>l+x), log:`Merge: compare ${left[i]} vs ${right[j]}` };
    a[k++] = left[i] <= right[j] ? left[i++] : right[j++];
    yield { arr:[...a], state:"active", active:Array.from({length:r-l+1},(_,x)=>l+x), log:`Place ${a[k-1]} at index ${k-1}` };
  }
  while (i < left.length) { a[k++] = left[i++]; yield { arr:[...a], state:"active", active:Array.from({length:r-l+1},(_,x)=>l+x), log:`Copy remaining left` }; }
  while (j < right.length) { a[k++] = right[j++]; yield { arr:[...a], state:"active", active:Array.from({length:r-l+1},(_,x)=>l+x), log:`Copy remaining right` }; }
}

async function* quickSortGen(arr) {
  const a = [...arr];
  yield* _quickSort(a, 0, a.length-1);
  yield { arr:[...a], state:"done", sorted:Array.from({length:a.length},(_,k)=>k), log:"Sorted." };
}
async function* _quickSort(a, l, r) {
  if (l >= r) return;
  let pivot = r, i = l;
  yield { arr:[...a], state:"pivot", indices:[pivot], active:Array.from({length:r-l+1},(_,x)=>l+x), log:`Pivot = a[${pivot}] = ${a[pivot]}` };
  for (let j = l; j < r; j++) {
    yield { arr:[...a], state:"comparing", indices:[j], pivot:[pivot], active:Array.from({length:r-l+1},(_,x)=>l+x), log:`Compare a[${j}]=${a[j]} < pivot=${a[pivot]}?` };
    if (a[j] < a[pivot]) { [a[i],a[j]] = [a[j],a[i]]; yield { arr:[...a], state:"swapped", indices:[i,j], log:`Swap a[${i}] <-> a[${j}]` }; i++; }
  }
  [a[i],a[r]] = [a[r],a[i]];
  yield { arr:[...a], state:"pivot", indices:[i], log:`Pivot settled at index ${i}` };
  yield* _quickSort(a, l, i-1);
  yield* _quickSort(a, i+1, r);
}

async function* linearSearchGen(arr, target) {
  for (let i = 0; i < arr.length; i++) {
    yield { arr:[...arr], state:"comparing", indices:[i], log:`Check a[${i}] = ${arr[i]} == ${target}?` };
    if (arr[i] === target) { yield { arr:[...arr], state:"found", indices:[i], log:`Found ${target} at index ${i}` }; return; }
  }
  yield { arr:[...arr], state:"done", indices:[], log:`${target} not found.` };
}

async function* binarySearchGen(arr, target) {
  let l=0, r=arr.length-1;
  while (l <= r) {
    const m = Math.floor((l+r)/2);
    yield { arr:[...arr], state:"comparing", indices:[m], lo:l, hi:r, log:`mid=${m}, a[${m}]=${arr[m]}, target=${target}` };
    if (arr[m] === target) { yield { arr:[...arr], state:"found", indices:[m], lo:l, hi:r, log:`Found at index ${m}` }; return; }
    if (arr[m] < target) { l=m+1; yield { arr:[...arr], state:"active", lo:l, hi:r, log:`Go right (lo=${l})` }; }
    else { r=m-1; yield { arr:[...arr], state:"active", lo:l, hi:r, log:`Go left (hi=${r})` }; }
  }
  yield { arr:[...arr], state:"done", log:`Not found.` };
}

async function* twoPointerGen(arr, target) {
  let l=0, r=arr.length-1;
  while (l < r) {
    const sum = arr[l]+arr[r];
    yield { arr:[...arr], state:"comparing", pA:l, pB:r, log:`a[${l}]=${arr[l]} + a[${r}]=${arr[r]} = ${sum}, target=${target}` };
    if (sum === target) { yield { arr:[...arr], state:"found", pA:l, pB:r, log:`Pair found: a[${l}]+a[${r}]=${target}` }; return; }
    if (sum < target) { l++; yield { arr:[...arr], pA:l, pB:r, log:`Sum too small, move left pointer right` }; }
    else { r--; yield { arr:[...arr], pA:l, pB:r, log:`Sum too large, move right pointer left` }; }
  }
  yield { arr:[...arr], state:"done", log:`No pair found summing to ${target}.` };
}

async function* slidingWindowGen(arr, k) {
  let sum = arr.slice(0, k).reduce((a,b)=>a+b, 0);
  let maxSum = sum, maxStart = 0;
  yield { arr:[...arr], windowStart:0, windowEnd:k-1, sum, maxSum, log:`Initial window [0..${k-1}] sum=${sum}` };
  for (let i = k; i < arr.length; i++) {
    sum = sum - arr[i-k] + arr[i];
    if (sum > maxSum) { maxSum = sum; maxStart = i-k+1; }
    yield { arr:[...arr], windowStart:i-k+1, windowEnd:i, sum, maxSum, log:`Window [${i-k+1}..${i}] sum=${sum}, max=${maxSum}` };
  }
  yield { arr:[...arr], windowStart:maxStart, windowEnd:maxStart+k-1, sum:maxSum, maxSum, state:"done", log:`Max sum = ${maxSum} at [${maxStart}..${maxStart+k-1}]` };
}

async function* floydCycleGen(list) {
  let slow=0, fast=0, step=0;
  const n = list.length;
  while (step < n*2) {
    slow = (slow+1) % n;
    fast = (fast+2) % n;
    step++;
    yield { list:[...list], slow, fast, log:`Step ${step}: slow@${slow}=${list[slow]}, fast@${fast}=${list[fast]}` };
    if (slow === fast) { yield { list:[...list], slow, fast, state:"found", log:`Cycle detected! slow=fast=${slow}` }; return; }
  }
  yield { list:[...list], state:"done", log:"No cycle detected." };
}

async function* heapifyGen(arr) {
  const a = [...arr], n = a.length;
  for (let i = Math.floor(n/2)-1; i >= 0; i--) {
    yield* _heapify(a, n, i);
  }
  yield { arr:[...a], state:"done", log:"Heap property satisfied." };
}
async function* _heapify(a, n, i) {
  let largest=i, l=2*i+1, r=2*i+2;
  yield { arr:[...a], indices:[i, l<n?l:i, r<n?r:i], log:`Heapify at ${i}: l=${l<n?a[l]:'—'}, r=${r<n?a[r]:'—'}` };
  if (l<n && a[l]>a[largest]) largest=l;
  if (r<n && a[r]>a[largest]) largest=r;
  if (largest!==i) { [a[i],a[largest]]=[a[largest],a[i]]; yield { arr:[...a], indices:[i,largest], state:"swapped", log:`Swap a[${i}] <-> a[${largest}]` }; yield* _heapify(a,n,largest); }
}

// ─── 3. PRIMITIVE COMPONENTS ──────────────────────────────────────────────────

function Btn({ variant="secondary", disabled, onClick, children, ariaLabel }) {
  return (
    <button
      className={`btn btn-${variant}`}
      disabled={disabled}
      onClick={onClick}
      aria-label={ariaLabel}
    >
      {children}
    </button>
  );
}

function StatBar({ label, value }) {
  return (
    <div className="info-stat">
      <span className="info-stat-label">{label}</span>
      <span className="info-stat-value">{value}</span>
    </div>
  );
}

function Legend({ items }) {
  return (
    <div className="legend" role="list" aria-label="Color legend">
      {items.map(([label, cls]) => (
        <div key={label} className="legend-item" role="listitem">
          <div className={`legend-dot sort-bar ${cls}`} style={{height:10}} />
          <span>{label}</span>
        </div>
      ))}
    </div>
  );
}

function StepLog({ entries }) {
  const ref = useRef(null);
  useEffect(() => { if (ref.current) ref.current.scrollTop = ref.current.scrollHeight; }, [entries]);
  return (
    <div className="step-log" ref={ref} aria-live="polite" aria-label="Step log">
      {entries.length === 0
        ? <span className="step-log-entry">Ready. Press Run to start.</span>
        : entries.map((e, i) => (
            <div key={i} className={`step-log-entry${i === entries.length-1 ? " current" : ""}`}>{e}</div>
          ))
      }
    </div>
  );
}

function EmptyState({ icon, title, body }) {
  return (
    <div className="empty-state" role="status">
      <div className="empty-state-icon" aria-hidden="true">{icon}</div>
      <p className="empty-state-title">{title}</p>
      <p className="empty-state-body">{body}</p>
    </div>
  );
}

function SkeletonBlock({ height = 200 }) {
  return <div className="skeleton" style={{height}} aria-hidden="true" />;
}

// ─── 4. VISUALIZER COMPONENTS ─────────────────────────────────────────────────

// ── 4a. Array/Sort Visualizer (shared by sort, search, two-pointer, sliding window)
function ArrayVisualizer({ frames, onRun, onReset, onSpeedChange, running, speed, title, subtitle, controls, showBars=false }) {
  const frame = frames[frames.length-1] || { arr:[], indices:[], sorted:[], active:[], pA:-1, pB:-1, windowStart:-1, windowEnd:-1 };
  const arr = frame.arr || [];
  const logs = frames.map(f => f.log).filter(Boolean);

  const getState = (i) => {
    if (frame.state === "done" && frame.sorted?.includes(i)) return "sorted";
    if (frame.state === "found" && frame.indices?.includes(i)) return "found";
    if (frame.pA === i) return "pointer-a";
    if (frame.pB === i) return "pointer-b";
    if (frame.windowStart !== undefined && i >= frame.windowStart && i <= frame.windowEnd) return "window";
    if (frame.pivot?.includes(i)) return "pivot";
    if (frame.indices?.includes(i) && frame.state === "swapped") return "swapped";
    if (frame.indices?.includes(i)) return "comparing";
    if (frame.active?.includes(i)) return "active";
    if (frame.sorted?.includes(i)) return "sorted";
    if (frame.lo !== undefined && (i < frame.lo || i > frame.hi)) return "active";
    return "idle";
  };

  const getLabel = (i) => {
    if (frame.pA === i) return "L";
    if (frame.pB === i) return "R";
    if (frame.pivot?.includes(i)) return "P";
    if (frame.lo !== undefined && i === frame.lo) return "lo";
    if (frame.hi !== undefined && i === frame.hi) return "hi";
    return "";
  };

  const maxVal = Math.max(...arr, 1);

  return (
    <section className="panel" aria-labelledby={`${title}-heading`}>
      <header className="panel-header">
        <div>
          <h2 id={`${title}-heading`} className="panel-title">{title}</h2>
          <p className="panel-subtitle">{subtitle}</p>
        </div>
        <div className="panel-controls">
          {controls}
          <Btn variant={running ? "danger" : "primary"} onClick={onRun}>{running ? "Stop" : "Run"}</Btn>
          <Btn variant="secondary" onClick={onReset} disabled={running}>Reset</Btn>
          <input className="range" type="range" min="1" max="100" value={speed} onChange={e => onSpeedChange(+e.target.value)} aria-label="Animation speed" style={{width:80}} />
        </div>
      </header>

      {arr.length === 0
        ? <SkeletonBlock height={200} />
        : showBars
          ? (
            <div className="sort-canvas" role="img" aria-label="Sorting visualization">
              {arr.map((v,i) => (
                <div key={i} className={`sort-bar ${getState(i)}`} style={{height: `${Math.max(2,(v/maxVal)*170)}px`}} aria-hidden="true" />
              ))}
            </div>
          )
          : (
            <div className="array-row" role="list" aria-label="Array elements" style={{flexWrap:"wrap", gap:"var(--sp-2)", alignItems:"flex-end"}}>
              {arr.map((v,i) => (
                <div key={i} className="array-cell" role="listitem">
                  <div className="array-cell-label">{getLabel(i)}</div>
                  <div className={`array-cell-box ${getState(i)}`}>{v}</div>
                  <div className="array-cell-idx">{i}</div>
                </div>
              ))}
            </div>
          )
      }

      <StepLog entries={logs} />
    </section>
  );
}

// ── 4b. Linked List Visualizer
function LinkedListVisualizer({ title, subtitle, doubly=false }) {
  const [nodes, setNodes] = useState([12, 27, 45, 8, 33]);
  const [input, setInput] = useState("");
  const [highlighted, setHighlighted] = useState(null);
  const [logs, setLogs] = useState([]);

  const addNode = () => {
    const v = parseInt(input);
    if (isNaN(v)) return;
    setNodes(n => [...n, v]);
    setLogs(l => [...l, `Appended node ${v} at tail`]);
    setInput("");
  };

  const removeNode = (i) => {
    setLogs(l => [...l, `Removed node ${nodes[i]} at index ${i}`]);
    setNodes(n => n.filter((_,idx) => idx !== i));
  };

  const W = 64, GAP = 40, R = 20;
  const total = nodes.length;
  const svgW = Math.max(400, total * (W + GAP) + GAP);

  return (
    <section className="panel" aria-labelledby={`${title}-heading`}>
      <header className="panel-header">
        <div>
          <h2 id={`${title}-heading`} className="panel-title">{title}</h2>
          <p className="panel-subtitle">{subtitle}</p>
        </div>
        <div className="panel-controls">
          <input className="input" type="number" value={input} onChange={e => setInput(e.target.value)} onKeyDown={e => e.key==="Enter"&&addNode()} placeholder="Value" aria-label="Node value" style={{width:80}} />
          <Btn variant="primary" onClick={addNode}>Append</Btn>
        </div>
      </header>

      {nodes.length === 0
        ? <EmptyState icon="—" title="Empty list" body="Append nodes above to visualize the linked list." />
        : (
          <svg className="svg-canvas" viewBox={`0 0 ${svgW} 80`} aria-label="Linked list visualization">
            {nodes.map((val, i) => {
              const cx = GAP + i*(W+GAP) + W/2;
              const isHi = highlighted === i;
              return (
                <g key={i} role="button" aria-label={`Node ${val}, index ${i}`} style={{cursor:"pointer"}} onClick={() => setHighlighted(i===highlighted?null:i)}>
                  <rect x={cx-W/2} y={20} width={W} height={40} rx={4} fill="var(--c-raised)" stroke={isHi?"var(--c-accent)":"var(--c-border)"} strokeWidth={isHi?2:1} />
                  <text x={cx} y={45} textAnchor="middle" fill={isHi?"var(--c-accent)":"var(--c-text)"} fontSize={12} fontFamily="var(--font-mono)" fontWeight="600">{val}</text>
                  {doubly && i > 0 && (
                    <path d={`M ${cx-W/2} 36 L ${cx-W/2-GAP} 36`} fill="none" stroke="var(--c-blue)" strokeWidth={1} markerEnd="url(#arr-back)" />
                  )}
                  {i < total-1 && (
                    <path d={`M ${cx+W/2} 40 L ${cx+W/2+GAP} 40`} fill="none" stroke="var(--c-border-hi)" strokeWidth={1} markerEnd="url(#arr-fwd)" />
                  )}
                  {i === total-1 && (
                    <text x={cx+W/2+6} y={44} fill="var(--c-text-3)" fontSize={10} fontFamily="var(--font-mono)">null</text>
                  )}
                  <text x={cx} y={18} textAnchor="middle" fill="var(--c-text-3)" fontSize={9} fontFamily="var(--font-mono)">[{i}]</text>
                  <g onClick={e => { e.stopPropagation(); removeNode(i); }} aria-label={`Remove node ${val}`} style={{cursor:"pointer"}}>
                    <rect x={cx+W/2-12} y={20} width={12} height={12} rx={2} fill="var(--c-red-dim)" stroke="var(--c-red)" strokeWidth={0.5} />
                    <text x={cx+W/2-6} y={29} textAnchor="middle" fill="var(--c-red)" fontSize={9} fontFamily="var(--font-mono)">x</text>
                  </g>
                </g>
              );
            })}
            <defs>
              <marker id="arr-fwd" viewBox="0 0 10 10" refX={8} refY={5} markerWidth={6} markerHeight={6} orient="auto">
                <path d="M2 2L8 5L2 8" fill="none" stroke="var(--c-border-hi)" strokeWidth={1.5} strokeLinecap="round" />
              </marker>
              <marker id="arr-back" viewBox="0 0 10 10" refX={2} refY={5} markerWidth={6} markerHeight={6} orient="auto-start-reverse">
                <path d="M8 2L2 5L8 8" fill="none" stroke="var(--c-blue)" strokeWidth={1.5} strokeLinecap="round" />
              </marker>
            </defs>
          </svg>
        )
      }
      <StepLog entries={logs} />
    </section>
  );
}

// ── 4c. Stack / Queue Visualizer
function StackQueueVisualizer({ title, subtitle, mode="stack" }) {
  const [items, setItems] = useState([]);
  const [input, setInput] = useState("");
  const [logs, setLogs] = useState([]);
  const [highlighted, setHighlighted] = useState(null);
  const MAX = 8;

  const push = () => {
    const v = input.trim(); if (!v) return;
    if (items.length >= MAX) { setLogs(l=>[...l,`Overflow: max ${MAX} items`]); return; }
    setItems(i => [...i, v]);
    setLogs(l => [...l, mode==="stack" ? `Push "${v}" onto stack` : `Enqueue "${v}"`]);
    setInput(""); setHighlighted(null);
  };

  const pop = () => {
    if (!items.length) { setLogs(l=>[...l,`Underflow: empty ${mode}`]); return; }
    const val = mode==="stack" ? items[items.length-1] : items[0];
    setItems(i => mode==="stack" ? i.slice(0,-1) : i.slice(1));
    setLogs(l => [...l, mode==="stack" ? `Pop "${val}" from stack` : `Dequeue "${val}"`]);
    setHighlighted(null);
  };

  return (
    <section className="panel" aria-labelledby={`${title}-heading`}>
      <header className="panel-header">
        <div>
          <h2 id={`${title}-heading`} className="panel-title">{title}</h2>
          <p className="panel-subtitle">{subtitle}</p>
        </div>
        <div className="panel-controls">
          <input className="input" type="text" value={input} onChange={e=>setInput(e.target.value)} onKeyDown={e=>e.key==="Enter"&&push()} placeholder="Value" aria-label="Item value" style={{width:80}} />
          <Btn variant="primary" onClick={push}>{mode==="stack"?"Push":"Enqueue"}</Btn>
          <Btn variant="secondary" onClick={pop}>{mode==="stack"?"Pop":"Dequeue"}</Btn>
        </div>
      </header>

      {items.length === 0
        ? <EmptyState icon="[ ]" title={`Empty ${mode}`} body={`${mode==="stack"?"Push":"Enqueue"} items to visualize.`} />
        : mode==="stack"
          ? (
            <div role="list" aria-label="Stack contents" style={{display:"flex",flexDirection:"column-reverse",gap:"var(--sp-1)",width:160}}>
              {items.map((val,i) => (
                <div key={i} role="listitem" className={`array-cell-box ${i===items.length-1?"comparing":"idle"}`} style={{width:"100%",justifyContent:"space-between",paddingLeft:"var(--sp-3)",paddingRight:"var(--sp-3)"}}>
                  <span>{val}</span>
                  {i===items.length-1 && <span style={{fontSize:9,color:"var(--c-accent)"}}>TOP</span>}
                </div>
              ))}
            </div>
          )
          : (
            <div style={{display:"flex",alignItems:"center",gap:"var(--sp-2)",flexWrap:"wrap"}} role="list" aria-label="Queue contents">
              <span style={{fontSize:10,color:"var(--c-text-3)"}}>FRONT</span>
              {items.map((val,i) => (
                <div key={i} role="listitem" className={`array-cell-box ${i===0?"comparing":i===items.length-1?"pointer-b":"idle"}`}>
                  {val}
                </div>
              ))}
              <span style={{fontSize:10,color:"var(--c-text-3)"}}>REAR</span>
            </div>
          )
      }
      <StepLog entries={logs} />
    </section>
  );
}

// ── 4d. Circular Queue
function CircularQueueVisualizer({ title, subtitle }) {
  const MAX = 8;
  const [queue, setQueue] = useState(Array(MAX).fill(null));
  const [front, setFront] = useState(0);
  const [rear, setRear] = useState(0);
  const [size, setSize] = useState(0);
  const [input, setInput] = useState("");
  const [logs, setLogs] = useState([]);

  const enqueue = () => {
    const v = input.trim(); if (!v) return;
    if (size === MAX) { setLogs(l=>[...l,"Queue full (overflow)"]); return; }
    setQueue(q => { const n=[...q]; n[rear]=v; return n; });
    setRear(r => (r+1)%MAX);
    setSize(s => s+1);
    setLogs(l=>[...l,`Enqueue "${v}" at slot ${rear}`]);
    setInput("");
  };

  const dequeue = () => {
    if (size===0) { setLogs(l=>[...l,"Queue empty (underflow)"]); return; }
    setLogs(l=>[...l,`Dequeue "${queue[front]}" from slot ${front}`]);
    setQueue(q => { const n=[...q]; n[front]=null; return n; });
    setFront(f=>(f+1)%MAX);
    setSize(s=>s-1);
  };

  const CX=160, CY=110, RADIUS=80;
  const cells = Array.from({length:MAX}, (_,i) => {
    const angle = (i/MAX)*2*Math.PI - Math.PI/2;
    return { x: CX + RADIUS*Math.cos(angle), y: CY + RADIUS*Math.sin(angle), val: queue[i], isFront: i===front&&size>0, isRear: i===(rear-1+MAX)%MAX&&size>0 };
  });

  return (
    <section className="panel" aria-labelledby={`${title}-heading`}>
      <header className="panel-header">
        <div>
          <h2 id={`${title}-heading`} className="panel-title">{title}</h2>
          <p className="panel-subtitle">{subtitle}</p>
        </div>
        <div className="panel-controls">
          <input className="input" type="text" value={input} onChange={e=>setInput(e.target.value)} onKeyDown={e=>e.key==="Enter"&&enqueue()} placeholder="Value" style={{width:80}} aria-label="Value to enqueue" />
          <Btn variant="primary" onClick={enqueue}>Enqueue</Btn>
          <Btn variant="secondary" onClick={dequeue}>Dequeue</Btn>
        </div>
      </header>

      <svg className="svg-canvas" viewBox="0 0 320 220" aria-label="Circular queue visualization">
        <circle cx={CX} cy={CY} r={RADIUS} fill="none" stroke="var(--c-border)" strokeWidth={1} strokeDasharray="4 4" />
        {cells.map((c,i) => (
          <g key={i}>
            <circle cx={c.x} cy={c.y} r={18} fill={c.val ? (c.isFront?"var(--c-green-dim)":c.isRear?"var(--c-blue-dim)":"var(--c-accent-dim)") : "var(--c-raised)"} stroke={c.val ? (c.isFront?"var(--c-green)":c.isRear?"var(--c-blue)":"var(--c-accent)") : "var(--c-border)"} strokeWidth={1.5} />
            <text x={c.x} y={c.y+4} textAnchor="middle" fill={c.val?"var(--c-text)":"var(--c-text-3)"} fontSize={11} fontFamily="var(--font-mono)" fontWeight="600">{c.val || i}</text>
            {c.isFront && <text x={c.x} y={c.y-23} textAnchor="middle" fill="var(--c-green)" fontSize={9} fontFamily="var(--font-mono)">F</text>}
            {c.isRear  && <text x={c.x} y={c.y-23} textAnchor="middle" fill="var(--c-blue)" fontSize={9} fontFamily="var(--font-mono)">R</text>}
          </g>
        ))}
        <text x={CX} y={CY-6} textAnchor="middle" fill="var(--c-text-3)" fontSize={10} fontFamily="var(--font-mono)">size</text>
        <text x={CX} y={CY+10} textAnchor="middle" fill="var(--c-accent)" fontSize={18} fontFamily="var(--font-mono)" fontWeight="700">{size}</text>
        <text x={250} y={30} fill="var(--c-green)" fontSize={10} fontFamily="var(--font-mono)">F = Front</text>
        <text x={250} y={46} fill="var(--c-blue)" fontSize={10} fontFamily="var(--font-mono)">R = Rear</text>
      </svg>

      <StepLog entries={logs} />
    </section>
  );
}

// ── 4e. Hash Table / Map / Collision
function HashVisualizer({ title, subtitle, mode="chaining" }) {
  const SIZE = 10;
  const [table, setTable] = useState(() => Array.from({length:SIZE}, ()=>[]));
  const [input, setInput] = useState("");
  const [logs, setLogs] = useState([]);
  const [probeSteps, setProbeSteps] = useState({});

  const hash = (key) => {
    if (typeof key === "string") return key.split("").reduce((a,c)=>a+c.charCodeAt(0),0) % SIZE;
    return Math.abs(parseInt(key)||0) % SIZE;
  };

  const insert = () => {
    const v = input.trim(); if (!v) return;
    const h = hash(v);
    if (mode === "chaining") {
      if (table[h].includes(v)) { setLogs(l=>[...l,`"${v}" already at slot ${h}`]); setInput(""); return; }
      setTable(t => { const n=t.map(r=>[...r]); n[h].push(v); return n; });
      setLogs(l=>[...l,`hash("${v}") = ${h} — ${table[h].length>0?"COLLISION, chained":"inserted"}`]);
    } else {
      let idx = h, step=0;
      const flat = table.map(r=>r[0]||null);
      while (flat[idx] !== null && flat[idx] !== v && step < SIZE) { idx=(idx+1)%SIZE; step++; }
      setTable(t => { const n=t.map(r=>[...r]); n[idx]=[v]; return n; });
      setProbeSteps(p => ({...p, [idx]: step}));
      setLogs(l=>[...l,`hash("${v}") = ${h}${step>0?` — collision, probed ${step} step(s) to slot ${idx}`:`— inserted at ${idx}`}`]);
    }
    setInput("");
  };

  const clear = () => { setTable(Array.from({length:SIZE},()=>[])); setLogs(l=>[...l,"Table cleared."]); setProbeSteps({}); };

  return (
    <section className="panel" aria-labelledby={`${title}-heading`}>
      <header className="panel-header">
        <div>
          <h2 id={`${title}-heading`} className="panel-title">{title}</h2>
          <p className="panel-subtitle">{subtitle}</p>
        </div>
        <div className="panel-controls">
          <select className="select" value={mode} onChange={()=>{}} aria-label="Collision mode" disabled>
            <option value="chaining">Chaining</option>
            <option value="linear">Linear Probe</option>
          </select>
          <input className="input" type="text" value={input} onChange={e=>setInput(e.target.value)} onKeyDown={e=>e.key==="Enter"&&insert()} placeholder="Key" style={{width:80}} aria-label="Key to insert" />
          <Btn variant="primary" onClick={insert}>Insert</Btn>
          <Btn variant="secondary" onClick={clear}>Clear</Btn>
        </div>
      </header>

      <div className="hash-table" role="list" aria-label="Hash table slots">
        {table.map((slot, i) => (
          <div key={i} className={`hash-slot ${slot.length>1?"collision":slot.length===1?"occupied":""}`} role="listitem">
            <span className="hash-slot-idx">[{i}]</span>
            <div className="hash-slot-chain">
              {slot.map((v,j) => <span key={j} className="hash-node">{v}</span>)}
              {slot.length === 0 && <span style={{color:"var(--c-text-3)",fontSize:11}}>empty</span>}
            </div>
            {probeSteps[i] > 0 && <span style={{marginLeft:"auto",fontSize:9,color:"var(--c-blue)"}}>+{probeSteps[i]} probe</span>}
          </div>
        ))}
      </div>

      <StepLog entries={logs} />
    </section>
  );
}

// ── 4f. BST / Binary Tree / AVL
function TreeVisualizer({ title, subtitle, mode="bst" }) {
  const [values, setValues] = useState([50,25,75,12,37,62,88]);
  const [input, setInput] = useState("");
  const [highlighted, setHighlighted] = useState(null);
  const [traversal, setTraversal] = useState([]);
  const [travIdx, setTravIdx] = useState(-1);
  const [logs, setLogs] = useState([]);

  class Node { constructor(v){this.v=v;this.l=null;this.r=null;this.h=1;} }

  const insert = (root, v) => {
    if (!root) return new Node(v);
    if (v < root.v) root.l = insert(root.l, v);
    else if (v > root.v) root.r = insert(root.r, v);
    return root;
  };

  const build = (vals) => vals.reduce((t,v) => insert(t?JSON.parse(JSON.stringify(t)):null,v), null);

  const layout = (node, x, y, dx) => {
    if (!node) return {nodes:[], edges:[]};
    const left = node.l ? layout(node.l, x-dx, y+64, dx/1.7) : {nodes:[],edges:[]};
    const right = node.r ? layout(node.r, x+dx, y+64, dx/1.7) : {nodes:[],edges:[]};
    const edges = [...left.edges, ...right.edges];
    if (node.l) edges.push({x1:x,y1:y,x2:x-dx,y2:y+64});
    if (node.r) edges.push({x1:x,y1:y,x2:x+dx,y2:y+64});
    return { nodes:[{v:node.v,x,y},...left.nodes,...right.nodes], edges };
  };

  const root = build(values);
  const { nodes, edges } = root ? layout(root, 300, 36, 120) : {nodes:[],edges:[]};

  const addNode = () => {
    const v = parseInt(input); if (isNaN(v)) return;
    setValues(a => [...a, v]);
    setLogs(l=>[...l,`Insert ${v} into ${mode.toUpperCase()}`]);
    setInput(""); setTraversal([]); setTravIdx(-1);
  };

  const inorder = (n, acc=[]) => { if(!n) return acc; inorder(n.l,acc); acc.push(n.v); inorder(n.r,acc); return acc; };
  const preorder = (n, acc=[]) => { if(!n) return acc; acc.push(n.v); preorder(n.l,acc); preorder(n.r,acc); return acc; };
  const postorder = (n, acc=[]) => { if(!n) return acc; postorder(n.l,acc); postorder(n.r,acc); acc.push(n.v); return acc; };

  const runTraversal = async (type) => {
    const fn = type==="in"?inorder:type==="pre"?preorder:postorder;
    const order = fn(root);
    setTraversal(order); setTravIdx(-1);
    setLogs(l=>[...l,`Starting ${type==="in"?"in":type==="pre"?"pre":"post"}-order traversal`]);
    for (let i=0; i<order.length; i++) {
      setTravIdx(i); setHighlighted(order[i]);
      setLogs(l=>[...l,`Visit ${order[i]}`]);
      await sleep(400);
    }
    setHighlighted(null);
    setLogs(l=>[...l,`Traversal: [${order.join(", ")}]`]);
  };

  return (
    <section className="panel" aria-labelledby={`${title}-heading`}>
      <header className="panel-header">
        <div>
          <h2 id={`${title}-heading`} className="panel-title">{title}</h2>
          <p className="panel-subtitle">{subtitle}</p>
        </div>
        <div className="panel-controls">
          <input className="input" type="number" value={input} onChange={e=>setInput(e.target.value)} onKeyDown={e=>e.key==="Enter"&&addNode()} placeholder="Value" style={{width:72}} aria-label="Node value" />
          <Btn variant="primary" onClick={addNode}>Insert</Btn>
          <Btn variant="ghost" onClick={()=>runTraversal("in")}>In-order</Btn>
          <Btn variant="ghost" onClick={()=>runTraversal("pre")}>Pre-order</Btn>
          <Btn variant="ghost" onClick={()=>runTraversal("post")}>Post-order</Btn>
          <Btn variant="secondary" onClick={()=>{setValues([50,25,75,12,37,62,88]);setTraversal([]);setTravIdx(-1);setHighlighted(null);}}>Reset</Btn>
        </div>
      </header>

      <svg className="svg-canvas" viewBox="0 0 600 300" aria-label="Binary search tree visualization">
        {edges.map((e,i) => (
          <line key={i} x1={e.x1} y1={e.y1} x2={e.x2} y2={e.y2} stroke="var(--c-border-hi)" strokeWidth={1.5} />
        ))}
        {nodes.map((n,i) => (
          <g key={i} role="button" tabIndex={0} aria-label={`Node ${n.v}`} style={{cursor:"pointer"}} onClick={()=>setHighlighted(n.v===highlighted?null:n.v)}>
            <circle cx={n.x} cy={n.y} r={19} fill={highlighted===n.v?"var(--c-accent-dim)":"var(--c-raised)"} stroke={highlighted===n.v?"var(--c-accent)":"var(--c-border-hi)"} strokeWidth={highlighted===n.v?2:1} className="tree-node-circle" />
            <text x={n.x} y={n.y+5} textAnchor="middle" fill={highlighted===n.v?"var(--c-accent)":"var(--c-text)"} fontSize={11} fontFamily="var(--font-mono)" fontWeight="700">{n.v}</text>
          </g>
        ))}
        {nodes.length===0 && <text x="300" y="155" textAnchor="middle" fill="var(--c-text-3)" fontSize={13} fontFamily="var(--font-mono)">Insert values to build the tree</text>}
      </svg>

      {traversal.length > 0 && (
        <div className="trie-word-list" role="list" aria-label="Traversal sequence">
          {traversal.map((v,i) => (
            <span key={i} role="listitem" className={`trie-word-tag${i===travIdx?" highlighted":""}`}>{v}</span>
          ))}
        </div>
      )}

      <StepLog entries={logs} />
    </section>
  );
}

// ── 4g. Heap / Priority Queue
function HeapVisualizer({ title, subtitle }) {
  const [heap, setHeap] = useState([90,75,80,55,60,65,70]);
  const [input, setInput] = useState("");
  const [highlighted, setHighlighted] = useState([]);
  const [logs, setLogs] = useState([]);
  const [running, setRunning] = useState(false);
  const [speed, setSpeed] = useState(50);
  const runRef = useRef(false);

  const parent = i => Math.floor((i-1)/2);
  const left = i => 2*i+1;
  const right = i => 2*i+2;

  const insertNode = () => {
    const v = parseInt(input); if (isNaN(v)) return;
    const h = [...heap, v];
    let i = h.length-1;
    while (i>0 && h[parent(i)]<h[i]) { [h[i],h[parent(i)]]=[h[parent(i)],h[i]]; i=parent(i); }
    setHeap(h); setInput(""); setLogs(l=>[...l,`Inserted ${v}, bubbled up`]);
  };

  const runHeapify = async () => {
    if (running) { runRef.current=false; return; }
    runRef.current=true; setRunning(true);
    const a=[...heap], n=a.length;
    const log = [];
    for (let i=Math.floor(n/2)-1; i>=0; i--) {
      await heapifyStep(a, n, i);
    }
    setHeap([...a]); setHighlighted([]); runRef.current=false; setRunning(false);
    setLogs(l=>[...l,"Heapify complete (max-heap property satisfied)"]);
  };

  const heapifyStep = async (a, n, i) => {
    let largest=i;
    const l=left(i), r=right(i);
    if (l<n && a[l]>a[largest]) largest=l;
    if (r<n && a[r]>a[largest]) largest=r;
    setHighlighted([i,l<n?l:-1,r<n?r:-1]);
    setLogs(prev=>[...prev,`Heapify at ${i}: checking children l=${l<n?a[l]:'—'} r=${r<n?a[r]:'—'}`]);
    await sleep(Math.max(80,300-speed*2));
    if (largest!==i) {
      [a[i],a[largest]]=[a[largest],a[i]];
      setHeap([...a]); setHighlighted([i,largest]);
      setLogs(prev=>[...prev,`Swap a[${i}]=${a[largest]} <-> a[${largest}]=${a[i]}`]);
      await sleep(Math.max(80,300-speed*2));
      await heapifyStep(a,n,largest);
    }
  };

  const extractMax = () => {
    if (!heap.length) return;
    const h=[...heap];
    setLogs(l=>[...l,`Extract max: ${h[0]}`]);
    h[0]=h[h.length-1]; h.pop();
    let i=0;
    while (true) {
      let largest=i;
      if (left(i)<h.length && h[left(i)]>h[largest]) largest=left(i);
      if (right(i)<h.length && h[right(i)]>h[largest]) largest=right(i);
      if (largest===i) break;
      [h[i],h[largest]]=[h[largest],h[i]]; i=largest;
    }
    setHeap(h); setHighlighted([]);
    setLogs(l=>[...l,"Sink-down complete"]);
  };

  // Tree layout for heap array
  const heapLayout = (arr) => {
    const nodes=[], edges=[];
    const positions = (i,x,y,dx) => {
      if(i>=arr.length) return;
      nodes.push({i,v:arr[i],x,y});
      if(left(i)<arr.length){ edges.push({x1:x,y1:y,x2:x-dx,y2:y+56}); positions(left(i),x-dx,y+56,dx/1.8); }
      if(right(i)<arr.length){ edges.push({x1:x,y1:y,x2:x+dx,y2:y+56}); positions(right(i),x+dx,y+56,dx/1.8); }
    };
    positions(0,300,30,110);
    return {nodes,edges};
  };

  const {nodes,edges} = heapLayout(heap);

  return (
    <section className="panel" aria-labelledby={`${title}-heading`}>
      <header className="panel-header">
        <div>
          <h2 id={`${title}-heading`} className="panel-title">{title}</h2>
          <p className="panel-subtitle">{subtitle}</p>
        </div>
        <div className="panel-controls">
          <input className="input" type="number" value={input} onChange={e=>setInput(e.target.value)} onKeyDown={e=>e.key==="Enter"&&insertNode()} placeholder="Value" style={{width:72}} aria-label="Node value" />
          <Btn variant="primary" onClick={insertNode}>Insert</Btn>
          <Btn variant="secondary" onClick={extractMax}>Extract Max</Btn>
          <Btn variant={running?"danger":"ghost"} onClick={runHeapify}>Heapify</Btn>
          <input className="range" type="range" min="1" max="100" value={speed} onChange={e=>setSpeed(+e.target.value)} aria-label="Speed" style={{width:64}} />
        </div>
      </header>

      <div className="array-row" style={{marginBottom:"var(--sp-4)"}}>
        {heap.map((v,i) => (
          <div key={i} className="array-cell">
            <div className={`array-cell-box ${highlighted.includes(i)?"comparing":"idle"}`}>{v}</div>
            <div className="array-cell-idx">{i}</div>
          </div>
        ))}
      </div>

      <svg className="svg-canvas" viewBox="0 0 600 240" aria-label="Heap tree visualization">
        {edges.map((e,i)=><line key={i} x1={e.x1} y1={e.y1} x2={e.x2} y2={e.y2} stroke="var(--c-border-hi)" strokeWidth={1.5}/>)}
        {nodes.map((n,i) => (
          <g key={i}>
            <circle cx={n.x} cy={n.y} r={18} fill={highlighted.includes(n.i)?"var(--c-accent-dim)":"var(--c-raised)"} stroke={highlighted.includes(n.i)?"var(--c-accent)":"var(--c-border-hi)"} strokeWidth={highlighted.includes(n.i)?2:1}/>
            <text x={n.x} y={n.y+5} textAnchor="middle" fill={highlighted.includes(n.i)?"var(--c-accent)":"var(--c-text)"} fontSize={11} fontFamily="var(--font-mono)" fontWeight="700">{n.v}</text>
          </g>
        ))}
      </svg>

      <StepLog entries={logs} />
    </section>
  );
}

// ── 4h. Trie
function TrieVisualizer({ title, subtitle }) {
  const [words, setWords] = useState(["apple","app","apt","bat","ball"]);
  const [input, setInput] = useState("");
  const [search, setSearch] = useState("");
  const [found, setFound] = useState(null);
  const [logs, setLogs] = useState([]);

  const addWord = () => {
    const w = input.trim().toLowerCase(); if(!w) return;
    if(words.includes(w)){ setLogs(l=>[...l,`"${w}" already in trie`]); return; }
    setWords(ws=>[...ws,w]); setLogs(l=>[...l,`Inserted "${w}" into trie`]); setInput("");
  };

  const searchWord = () => {
    const w = search.trim().toLowerCase();
    const res = words.includes(w);
    setFound(w);
    setLogs(l=>[...l,`Search "${w}": ${res?"FOUND":"NOT FOUND"}`]);
  };

  // Simple trie SVG — show character-by-character breakdown
  const buildTrieNodes = () => {
    const root = {};
    words.forEach(word => {
      let cur = root;
      for (const ch of word) {
        if (!cur[ch]) cur[ch] = {};
        cur = cur[ch];
      }
      cur["$"] = true;
    });
    return root;
  };

  const trieRoot = buildTrieNodes();

  // BFS layout
  const bfsLayout = () => {
    const nodes = [{ key:"root", label:"*", x:300, y:20, depth:0 }];
    const edges = [];
    const queue = [{ node:trieRoot, parentX:300, parentY:20, depth:1, prefix:"" }];
    const xOffsets = {};
    while (queue.length) {
      const { node, parentX, parentY, depth, prefix } = queue.shift();
      const children = Object.entries(node).filter(([k])=>k!=="$");
      if (!children.length) continue;
      xOffsets[depth] = xOffsets[depth] || 0;
      const startX = parentX - (children.length-1) * 28;
      children.forEach(([ch, child], ci) => {
        const x = startX + ci*56;
        const y = depth*52+20;
        const isFound = found && (prefix+ch) === found.slice(0, prefix.length+1);
        nodes.push({ key:prefix+ch, label:ch, x, y, depth, isEnd:child["$"]===true, isFound });
        edges.push({ x1:parentX, y1:parentY, x2:x, y2:y, label:ch });
        queue.push({ node:child, parentX:x, parentY:y, depth:depth+1, prefix:prefix+ch });
      });
    }
    return {nodes,edges};
  };

  const {nodes:tNodes, edges:tEdges} = bfsLayout();
  const svgH = Math.max(120, (Math.max(...tNodes.map(n=>n.depth),0)+1)*52+40);

  return (
    <section className="panel" aria-labelledby={`${title}-heading`}>
      <header className="panel-header">
        <div>
          <h2 id={`${title}-heading`} className="panel-title">{title}</h2>
          <p className="panel-subtitle">{subtitle}</p>
        </div>
        <div className="panel-controls">
          <input className="input" type="text" value={input} onChange={e=>setInput(e.target.value)} onKeyDown={e=>e.key==="Enter"&&addWord()} placeholder="Insert word" style={{width:100}} aria-label="Word to insert" />
          <Btn variant="primary" onClick={addWord}>Insert</Btn>
          <input className="input" type="text" value={search} onChange={e=>setSearch(e.target.value)} placeholder="Search" style={{width:88}} aria-label="Word to search" />
          <Btn variant="secondary" onClick={searchWord}>Search</Btn>
        </div>
      </header>

      <svg className="svg-canvas" viewBox={`0 0 600 ${svgH}`} aria-label="Trie visualization">
        {tEdges.map((e,i)=>(
          <line key={i} x1={e.x1} y1={e.y1} x2={e.x2} y2={e.y2} stroke="var(--c-border-hi)" strokeWidth={1}/>
        ))}
        {tNodes.map((n,i)=>(
          <g key={n.key}>
            <circle cx={n.x} cy={n.y} r={14} fill={n.isFound?"var(--c-accent-dim)":n.isEnd?"var(--c-green-dim)":"var(--c-raised)"} stroke={n.isFound?"var(--c-accent)":n.isEnd?"var(--c-green)":"var(--c-border-hi)"} strokeWidth={1.5}/>
            <text x={n.x} y={n.y+5} textAnchor="middle" fill={n.isFound?"var(--c-accent)":n.isEnd?"var(--c-green)":"var(--c-text)"} fontSize={11} fontFamily="var(--font-mono)" fontWeight="700">{n.label}</text>
          </g>
        ))}
      </svg>

      <div className="trie-word-list" role="list" aria-label="Words in trie">
        {words.map(w=>(
          <span key={w} role="listitem" className={`trie-word-tag${found===w?" highlighted":""}`}>{w}</span>
        ))}
      </div>

      <StepLog entries={logs} />
    </section>
  );
}

// ── 4i. Graph Visualizer (Directed/Undirected/Weighted + BFS/DFS/Dijkstra/A*/Kruskal/Prim/Topo)
function GraphVisualizer({ title, subtitle, mode="undirected" }) {
  const defaultNodes = [
    {id:0,x:120,y:80,label:"A"},{id:1,x:280,y:60,label:"B"},{id:2,x:420,y:100,label:"C"},
    {id:3,x:160,y:200,label:"D"},{id:4,x:320,y:180,label:"E"},{id:5,x:460,y:220,label:"F"},
  ];
  const defaultEdges = mode==="weighted"
    ? [{u:0,v:1,w:4},{u:0,v:3,w:2},{u:1,v:2,w:5},{u:1,v:4,w:3},{u:2,v:5,w:1},{u:3,v:4,w:6},{u:4,v:5,w:2}]
    : [{u:0,v:1},{u:0,v:3},{u:1,v:2},{u:1,v:4},{u:2,v:5},{u:3,v:4},{u:4,v:5}];

  const [nodes] = useState(defaultNodes);
  const [edges] = useState(defaultEdges);
  const [visited, setVisited] = useState(new Set());
  const [active, setActive] = useState(-1);
  const [edgeState, setEdgeState] = useState({});
  const [algo, setAlgo] = useState("bfs");
  const [running, setRunning] = useState(false);
  const [logs, setLogs] = useState([]);
  const [mstEdges, setMstEdges] = useState(new Set());
  const runRef = useRef(false);

  const adj = (id) => edges.filter(e => e.u===id || (mode!=="directed"&&e.v===id)).map(e=>e.u===id?e.v:e.u);

  const runBFS = async () => {
    const vis=new Set(), logArr=[], queue=[0];
    vis.add(0); setVisited(new Set(vis));
    while (queue.length && runRef.current) {
      const cur = queue.shift();
      setActive(cur); logArr.push(`Visit ${nodes[cur].label}`); setLogs([...logArr]);
      await sleep(400);
      for (const nb of adj(cur)) {
        if (!vis.has(nb)) { vis.add(nb); setVisited(new Set(vis)); queue.push(nb); logArr.push(`Enqueue ${nodes[nb].label}`); setLogs([...logArr]); }
      }
    }
    logArr.push("BFS complete."); setLogs([...logArr]);
  };

  const runDFS = async () => {
    const vis=new Set(), logArr=[];
    const dfs = async (u) => {
      if (!runRef.current) return;
      vis.add(u); setVisited(new Set(vis)); setActive(u);
      logArr.push(`Visit ${nodes[u].label}`); setLogs([...logArr]);
      await sleep(400);
      for (const nb of adj(u)) { if (!vis.has(nb)) await dfs(nb); }
    };
    await dfs(0); logArr.push("DFS complete."); setLogs([...logArr]);
  };

  const runDijkstra = async () => {
    const dist = Object.fromEntries(nodes.map(n=>[n.id,Infinity]));
    dist[0]=0; const vis=new Set(), logArr=[], edgeS={};
    while (vis.size < nodes.length && runRef.current) {
      const u = nodes.filter(n=>!vis.has(n.id)).sort((a,b)=>dist[a.id]-dist[b.id])[0];
      if (!u || dist[u.id]===Infinity) break;
      vis.add(u.id); setVisited(new Set(vis)); setActive(u.id);
      logArr.push(`Settle ${u.label}, dist=${dist[u.id]}`); setLogs([...logArr]);
      await sleep(400);
      edges.filter(e=>e.u===u.id||(mode!=="directed"&&e.v===u.id)).forEach(e=>{
        const nb=e.u===u.id?e.v:e.u, w=e.w||1;
        if (dist[u.id]+w < dist[nb]) { dist[nb]=dist[u.id]+w; edgeS[`${u.id}-${nb}`]="active"; setEdgeState({...edgeS}); logArr.push(`Relax ${nodes[nb].label} = ${dist[nb]}`); setLogs([...logArr]); }
      });
    }
    logArr.push("Dijkstra complete."); setLogs([...logArr]);
  };

  const runKruskal = async () => {
    const parent=nodes.map(n=>n.id), logArr=[];
    const find = (x) => { while(parent[x]!==x){parent[x]=parent[parent[x]];x=parent[x];} return x; };
    const union = (x,y) => { parent[find(x)]=find(y); };
    const sorted=[...edges].sort((a,b)=>(a.w||1)-(b.w||1));
    const mst=new Set();
    for (const e of sorted) {
      if (!runRef.current) break;
      if (find(e.u)!==find(e.v)) {
        union(e.u,e.v); mst.add(`${e.u}-${e.v}`); setMstEdges(new Set(mst));
        logArr.push(`Add edge ${nodes[e.u].label}-${nodes[e.v].label} w=${e.w||1}`); setLogs([...logArr]);
        await sleep(500);
      }
    }
    logArr.push("Kruskal MST complete."); setLogs([...logArr]);
  };

  const runPrim = async () => {
    const inMST=new Set([0]), mst=new Set(), logArr=[];
    setVisited(new Set(inMST));
    while (inMST.size < nodes.length && runRef.current) {
      const candidates=edges.filter(e=>(inMST.has(e.u)&&!inMST.has(e.v))||(mode!=="directed"&&inMST.has(e.v)&&!inMST.has(e.u)));
      if (!candidates.length) break;
      const best=candidates.sort((a,b)=>(a.w||1)-(b.w||1))[0];
      const nb=inMST.has(best.u)?best.v:best.u;
      inMST.add(nb); setVisited(new Set(inMST)); setActive(nb);
      mst.add(`${best.u}-${best.v}`); setMstEdges(new Set(mst));
      logArr.push(`Add ${nodes[best.u].label}-${nodes[best.v].label} w=${best.w||1}`); setLogs([...logArr]);
      await sleep(500);
    }
    logArr.push("Prim MST complete."); setLogs([...logArr]);
  };

  const runTopological = async () => {
    const inDegree=Object.fromEntries(nodes.map(n=>[n.id,0]));
    edges.forEach(e=>inDegree[e.v]=(inDegree[e.v]||0)+1);
    const queue=nodes.filter(n=>inDegree[n.id]===0).map(n=>n.id);
    const vis=new Set(), logArr=[];
    while (queue.length && runRef.current) {
      const u=queue.shift(); vis.add(u); setVisited(new Set(vis)); setActive(u);
      logArr.push(`Process ${nodes[u].label}`); setLogs([...logArr]);
      await sleep(400);
      edges.filter(e=>e.u===u).forEach(e=>{ inDegree[e.v]--; if(inDegree[e.v]===0) queue.push(e.v); });
    }
    logArr.push("Topological order complete."); setLogs([...logArr]);
  };

  const runAlgo = async () => {
    if (running) { runRef.current=false; return; }
    runRef.current=true; setRunning(true);
    setVisited(new Set()); setActive(-1); setEdgeState({}); setMstEdges(new Set()); setLogs([]);
    const fns = { bfs:runBFS, dfs:runDFS, dijkstra:runDijkstra, kruskal:runKruskal, prim:runPrim, topological:runTopological };
    await (fns[algo] || runBFS)();
    setActive(-1); runRef.current=false; setRunning(false);
  };

  const getNodeFill = (id) => {
    if (id===active) return "var(--c-accent-dim)";
    if (visited.has(id)) return "var(--c-blue-dim)";
    return "var(--c-raised)";
  };
  const getNodeStroke = (id) => {
    if (id===active) return "var(--c-accent)";
    if (visited.has(id)) return "var(--c-blue)";
    return "var(--c-border-hi)";
  };
  const getEdgeStroke = (e) => {
    if (mstEdges.has(`${e.u}-${e.v}`)||mstEdges.has(`${e.v}-${e.u}`)) return "var(--c-green)";
    if (edgeState[`${e.u}-${e.v}`]==="active") return "var(--c-accent)";
    return "var(--c-border-hi)";
  };

  const algoOptions = mode==="weighted"
    ? [{v:"dijkstra",l:"Dijkstra"},{v:"kruskal",l:"Kruskal MST"},{v:"prim",l:"Prim MST"}]
    : mode==="directed"
      ? [{v:"bfs",l:"BFS"},{v:"dfs",l:"DFS"},{v:"topological",l:"Topological Sort"}]
      : [{v:"bfs",l:"BFS"},{v:"dfs",l:"DFS"}];

  return (
    <section className="panel" aria-labelledby={`${title}-heading`}>
      <header className="panel-header">
        <div>
          <h2 id={`${title}-heading`} className="panel-title">{title}</h2>
          <p className="panel-subtitle">{subtitle}</p>
        </div>
        <div className="panel-controls">
          <select className="select" value={algo} onChange={e=>{setAlgo(e.target.value);setVisited(new Set());setActive(-1);setMstEdges(new Set());}} aria-label="Algorithm">
            {algoOptions.map(o=><option key={o.v} value={o.v}>{o.l}</option>)}
          </select>
          <Btn variant={running?"danger":"primary"} onClick={runAlgo}>{running?"Stop":"Run"}</Btn>
          <Btn variant="secondary" onClick={()=>{setVisited(new Set());setActive(-1);setEdgeState({});setMstEdges(new Set());setLogs([]);}} disabled={running}>Reset</Btn>
        </div>
      </header>

      <svg className="svg-canvas" viewBox="0 0 580 280" aria-label="Graph visualization">
        <defs>
          <marker id="g-arrow" viewBox="0 0 10 10" refX={8} refY={5} markerWidth={6} markerHeight={6} orient="auto">
            <path d="M2 2L8 5L2 8" fill="none" stroke="var(--c-border-hi)" strokeWidth={1.5} strokeLinecap="round"/>
          </marker>
        </defs>
        {edges.map((e,i)=>{
          const u=nodes[e.u], v=nodes[e.v];
          const mx=(u.x+v.x)/2, my=(u.y+v.y)/2;
          return (
            <g key={i}>
              <line x1={u.x} y1={u.y} x2={v.x} y2={v.y} stroke={getEdgeStroke(e)} strokeWidth={mstEdges.has(`${e.u}-${e.v}`)||mstEdges.has(`${e.v}-${e.u}`)?2.5:1.5} markerEnd={mode==="directed"?"url(#g-arrow)":undefined} />
              {e.w !== undefined && <text x={mx} y={my-6} textAnchor="middle" fill="var(--c-text-3)" fontSize={10} fontFamily="var(--font-mono)">{e.w}</text>}
            </g>
          );
        })}
        {nodes.map(n=>(
          <g key={n.id}>
            <circle cx={n.x} cy={n.y} r={20} fill={getNodeFill(n.id)} stroke={getNodeStroke(n.id)} strokeWidth={1.5}/>
            <text x={n.x} y={n.y+5} textAnchor="middle" fill={active===n.id?"var(--c-accent)":visited.has(n.id)?"var(--c-blue)":"var(--c-text)"} fontSize={12} fontFamily="var(--font-mono)" fontWeight="700">{n.label}</text>
          </g>
        ))}
      </svg>

      <StepLog entries={logs} />
    </section>
  );
}

// ── 4j. Union-Find / Path Compression
function UnionFindVisualizer({ title, subtitle }) {
  const N = 8;
  const [parent, setParent] = useState(Array.from({length:N},(_,i)=>i));
  const [rank, setRank] = useState(Array(N).fill(0));
  const [uA, setUA] = useState("0");
  const [uB, setUB] = useState("1");
  const [logs, setLogs] = useState([]);
  const [highlighted, setHighlighted] = useState([]);

  const find = (p, x) => { while(p[x]!==x){p[x]=p[p[x]];x=p[x];} return x; };

  const union = () => {
    const a=parseInt(uA), b=parseInt(uB);
    if (isNaN(a)||isNaN(b)||a>=N||b>=N) return;
    const p=[...parent], r=[...rank];
    const ra=find(p,a), rb=find(p,b);
    if (ra===rb){ setLogs(l=>[...l,`${a} and ${b} already in same set (root=${ra})`]); return; }
    if (r[ra]<r[rb]) p[ra]=rb; else if (r[ra]>r[rb]) p[rb]=ra; else { p[rb]=ra; r[ra]++; }
    setParent(p); setRank(r);
    setHighlighted([a,b,ra,rb]);
    setLogs(l=>[...l,`Union(${a},${b}): merged roots ${ra} and ${rb}`]);
  };

  const pathCompress = () => {
    const x=parseInt(uA); if(isNaN(x)||x>=N) return;
    const p=[...parent]; const root=find(p,x);
    setParent(p); setHighlighted([x,root]);
    setLogs(l=>[...l,`Path compress from ${x}: root=${root}, path flattened`]);
  };

  const reset = () => { setParent(Array.from({length:N},(_,i)=>i)); setRank(Array(N).fill(0)); setHighlighted([]); setLogs(l=>[...l,"Reset."]); };

  return (
    <section className="panel" aria-labelledby={`${title}-heading`}>
      <header className="panel-header">
        <div>
          <h2 id={`${title}-heading`} className="panel-title">{title}</h2>
          <p className="panel-subtitle">{subtitle}</p>
        </div>
        <div className="panel-controls">
          <input className="input" type="number" min="0" max={N-1} value={uA} onChange={e=>setUA(e.target.value)} style={{width:52}} aria-label="Node A" placeholder="A" />
          <input className="input" type="number" min="0" max={N-1} value={uB} onChange={e=>setUB(e.target.value)} style={{width:52}} aria-label="Node B" placeholder="B" />
          <Btn variant="primary" onClick={union}>Union</Btn>
          <Btn variant="secondary" onClick={pathCompress}>Find + Compress</Btn>
          <Btn variant="ghost" onClick={reset}>Reset</Btn>
        </div>
      </header>

      <div className="array-row" style={{marginBottom:"var(--sp-6)"}}>
        {parent.map((p,i) => (
          <div key={i} className="array-cell">
            <div className={`array-cell-box ${highlighted.includes(i)?"comparing":"idle"}`}>{p}</div>
            <div className="array-cell-idx">{i}</div>
          </div>
        ))}
      </div>

      <svg className="svg-canvas" viewBox="0 0 580 120" aria-label="Union-Find forest">
        {parent.map((p,i) => {
          if (p===i) return null;
          const x1=40+i*66, y1=50, x2=40+p*66, y2=50;
          return <path key={i} d={`M${x1} ${y1} C${x1} ${y1-40} ${x2} ${y2-40} ${x2} ${y2}`} fill="none" stroke={highlighted.includes(i)?"var(--c-accent)":"var(--c-border-hi)"} strokeWidth={1.5} markerEnd="url(#arr-fwd)"/>;
        })}
        {Array.from({length:N},(_,i)=>(
          <g key={i}>
            <circle cx={40+i*66} cy={50} r={18} fill={highlighted.includes(i)?"var(--c-accent-dim)":parent[i]===i?"var(--c-green-dim)":"var(--c-raised)"} stroke={highlighted.includes(i)?"var(--c-accent)":parent[i]===i?"var(--c-green)":"var(--c-border-hi)"} strokeWidth={1.5}/>
            <text x={40+i*66} y={55} textAnchor="middle" fill={parent[i]===i?"var(--c-green)":"var(--c-text)"} fontSize={12} fontFamily="var(--font-mono)" fontWeight="700">{i}</text>
            {parent[i]===i && <text x={40+i*66} y={80} textAnchor="middle" fill="var(--c-green)" fontSize={9} fontFamily="var(--font-mono)">root</text>}
          </g>
        ))}
      </svg>

      <StepLog entries={logs} />
    </section>
  );
}

// ── 4k. Pathfinding Grid (Dijkstra / A*)
const PF_ROWS=16, PF_COLS=28;
function PathfindingVisualizer({ title, subtitle }) {
  const [grid, setGrid] = useState(()=>Array.from({length:PF_ROWS},()=>Array(PF_COLS).fill(0)));
  const [start, setStart] = useState({r:3,c:3});
  const [end, setEnd] = useState({r:12,c:24});
  const [mode, setMode] = useState("wall");
  const [visited, setVisited] = useState(new Set());
  const [path, setPath] = useState(new Set());
  const [algo, setAlgo] = useState("astar");
  const [running, setRunning] = useState(false);
  const [logs, setLogs] = useState([]);
  const runRef = useRef(false);
  const drawRef = useRef(false);

  const key = (r,c) => `${r},${c}`;
  const nbrs = (r,c,g) => [[0,1],[0,-1],[1,0],[-1,0]].map(([dr,dc])=>[r+dr,c+dc]).filter(([nr,nc])=>nr>=0&&nr<PF_ROWS&&nc>=0&&nc<PF_COLS&&!g[nr][nc]);
  const h = (r,c) => Math.abs(r-end.r)+Math.abs(c-end.c);

  const runPath = async () => {
    if (running) { runRef.current=false; return; }
    runRef.current=true; setRunning(true);
    setVisited(new Set()); setPath(new Set()); setLogs([]);
    const vis=new Set(), prev={};
    let found=false;

    if (algo==="dijkstra") {
      const dist={[key(start.r,start.c)]:0};
      const pq=[[0,start.r,start.c]];
      while (pq.length && runRef.current) {
        pq.sort((a,b)=>a[0]-b[0]);
        const [d,r,c]=pq.shift(); const k=key(r,c);
        if (vis.has(k)) continue;
        vis.add(k); setVisited(new Set(vis));
        setLogs(l=>[...l,`Visit (${r},${c}) dist=${d}`]);
        await sleep(14);
        if (r===end.r&&c===end.c){found=true;break;}
        for (const [nr,nc] of nbrs(r,c,grid)) {
          const nk=key(nr,nc), nd=d+1;
          if (nd<(dist[nk]??Infinity)){dist[nk]=nd;prev[nk]=k;pq.push([nd,nr,nc]);}
        }
      }
    } else {
      const open=new Map([[key(start.r,start.c),[start.r,start.c]]]);
      const g={[key(start.r,start.c)]:0}, f={[key(start.r,start.c)]:h(start.r,start.c)};
      while (open.size && runRef.current) {
        let curK=null;
        for (const k of open.keys()) if(!curK||(f[k]??Infinity)<(f[curK]??Infinity)) curK=k;
        const [r,c]=open.get(curK); open.delete(curK);
        vis.add(curK); setVisited(new Set(vis));
        setLogs(l=>[...l,`Visit (${r},${c}) f=${(f[curK]||0).toFixed(1)}`]);
        await sleep(11);
        if (r===end.r&&c===end.c){found=true;break;}
        for (const [nr,nc] of nbrs(r,c,grid)) {
          const nk=key(nr,nc); if(vis.has(nk)) continue;
          const ng=(g[curK]??0)+1;
          if(ng<(g[nk]??Infinity)){g[nk]=ng;f[nk]=ng+h(nr,nc);prev[nk]=curK;open.set(nk,[nr,nc]);}
        }
      }
    }

    if (found) {
      const p=new Set(); let cur=key(end.r,end.c);
      while (cur&&cur!==key(start.r,start.c)){p.add(cur);cur=prev[cur];}
      p.add(key(start.r,start.c));
      setPath(p);
      setLogs(l=>[...l,`Path found! Length = ${p.size} cells`]);
    } else {
      setLogs(l=>[...l,"No path found."]);
    }
    runRef.current=false; setRunning(false);
  };

  const cellClass = (r,c) => {
    const k=key(r,c);
    if (r===start.r&&c===start.c) return "pf-cell start";
    if (r===end.r&&c===end.c) return "pf-cell end";
    if (grid[r][c]) return "pf-cell wall";
    if (path.has(k)) return "pf-cell path";
    if (visited.has(k)) return "pf-cell visited";
    return "pf-cell empty";
  };

  const handleCell = (r,c) => {
    if (running) return;
    setVisited(new Set()); setPath(new Set());
    if (mode==="start"){setStart({r,c});return;}
    if (mode==="end"){setEnd({r,c});return;}
    setGrid(g=>{const n=g.map(row=>[...row]);n[r][c]=n[r][c]?0:1;return n;});
  };

  return (
    <section className="panel" aria-labelledby={`${title}-heading`}>
      <header className="panel-header">
        <div>
          <h2 id={`${title}-heading`} className="panel-title">{title}</h2>
          <p className="panel-subtitle">{subtitle}</p>
        </div>
        <div className="panel-controls">
          {["wall","start","end"].map(m=>(
            <Btn key={m} variant={mode===m?"primary":"secondary"} onClick={()=>setMode(m)}>{m.toUpperCase()}</Btn>
          ))}
          <select className="select" value={algo} onChange={e=>{setAlgo(e.target.value);setVisited(new Set());setPath(new Set());}} aria-label="Algorithm">
            <option value="astar">A*</option>
            <option value="dijkstra">Dijkstra</option>
          </select>
          <Btn variant={running?"danger":"primary"} onClick={runPath}>{running?"Stop":"Run"}</Btn>
          <Btn variant="secondary" onClick={()=>{setGrid(Array.from({length:PF_ROWS},()=>Array(PF_COLS).fill(0)));setVisited(new Set());setPath(new Set());}} disabled={running}>Clear</Btn>
        </div>
      </header>

      <div className="pf-grid" style={{gridTemplateColumns:`repeat(${PF_COLS},1fr)`}}
        onMouseDown={()=>drawRef.current=true}
        onMouseUp={()=>drawRef.current=false}
        onMouseLeave={()=>drawRef.current=false}
        role="grid" aria-label="Pathfinding grid"
      >
        {Array.from({length:PF_ROWS},(_,r)=>
          Array.from({length:PF_COLS},(_,c)=>(
            <div key={`${r}-${c}`}
              className={cellClass(r,c)}
              onMouseDown={()=>handleCell(r,c)}
              onMouseEnter={()=>{if(drawRef.current&&mode==="wall"){setGrid(g=>{const n=g.map(row=>[...row]);n[r][c]=1;return n;});setVisited(new Set());setPath(new Set());}}}
              role="gridcell"
              aria-label={`Cell ${r},${c}`}
            />
          ))
        )}
      </div>

      <div className="legend">
        {[["Start","pf-cell start"],["End","pf-cell end"],["Wall","pf-cell wall"],["Visited","pf-cell visited"],["Path","pf-cell path"]].map(([l,cls])=>(
          <div key={l} className="legend-item">
            <div className={`legend-dot ${cls}`} style={{width:12,height:12}}/>
            <span>{l}</span>
          </div>
        ))}
      </div>

      <StepLog entries={logs} />
    </section>
  );
}

// ── 4l. Matrix Multiplication
function MatrixVisualizer({ title, subtitle }) {
  const [size, setSize] = useState(3);
  const mkRand = (s) => Array.from({length:s},()=>Array.from({length:s},()=>Math.floor(Math.random()*9)+1));
  const [A, setA] = useState(()=>mkRand(3));
  const [B, setB] = useState(()=>mkRand(3));
  const [C, setC] = useState(null);
  const [activeCell, setActiveCell] = useState(null);
  const [activeRow, setActiveRow] = useState(-1);
  const [activeCol, setActiveCol] = useState(-1);
  const [running, setRunning] = useState(false);
  const [logs, setLogs] = useState([]);
  const runRef = useRef(false);

  const regen = () => { const s=size; setA(mkRand(s)); setB(mkRand(s)); setC(null); setLogs([]); };

  const multiply = async () => {
    if (running){runRef.current=false;return;}
    runRef.current=true; setRunning(true);
    const n=A.length, result=Array.from({length:n},()=>Array(n).fill(0));
    for (let i=0;i<n&&runRef.current;i++) {
      for (let j=0;j<n&&runRef.current;j++) {
        setActiveRow(i); setActiveCol(j);
        let sum=0;
        for (let k=0;k<n&&runRef.current;k++) {
          sum+=A[i][k]*B[k][j];
          setActiveCell({i,j});
          setLogs(l=>[...l,`C[${i}][${j}] += A[${i}][${k}](${A[i][k]}) * B[${k}][${j}](${B[k][j]}) = ${sum}`]);
          await sleep(120);
        }
        result[i][j]=sum;
        setC(result.map(r=>[...r]));
      }
    }
    setActiveCell(null); setActiveRow(-1); setActiveCol(-1);
    setLogs(l=>[...l,"Matrix multiplication complete."]);
    runRef.current=false; setRunning(false);
  };

  const matClass = (type,r,c) => {
    if (type==="A" && activeRow===r) return "row-hi";
    if (type==="B" && activeCol===c) return "col-hi";
    if (type==="C" && activeCell?.i===r && activeCell?.j===c) return "active";
    if (type==="C" && C?.[r]?.[c] !== undefined && !running) return "result";
    return "";
  };

  const MatGrid = ({data,type,label}) => (
    <div>
      <div style={{fontSize:10,color:"var(--c-text-3)",marginBottom:"var(--sp-2)",letterSpacing:1}}>{label}</div>
      <div className="matrix-grid" style={{gridTemplateColumns:`repeat(${data.length},44px)`}}>
        {data.map((row,r)=>row.map((v,c)=>(
          <div key={`${r}-${c}`} className={`matrix-cell ${matClass(type,r,c)}`}>{v??""}</div>
        )))}
      </div>
    </div>
  );

  return (
    <section className="panel" aria-labelledby={`${title}-heading`}>
      <header className="panel-header">
        <div>
          <h2 id={`${title}-heading`} className="panel-title">{title}</h2>
          <p className="panel-subtitle">{subtitle}</p>
        </div>
        <div className="panel-controls">
          <select className="select" value={size} onChange={e=>{setSize(+e.target.value);setC(null);}} aria-label="Matrix size">
            <option value={2}>2x2</option><option value={3}>3x3</option><option value={4}>4x4</option>
          </select>
          <Btn variant="secondary" onClick={regen} disabled={running}>Randomize</Btn>
          <Btn variant={running?"danger":"primary"} onClick={multiply}>{running?"Stop":"Multiply"}</Btn>
        </div>
      </header>

      <div style={{display:"flex",alignItems:"flex-start",gap:"var(--sp-6)",flexWrap:"wrap"}}>
        <MatGrid data={A} type="A" label="MATRIX A" />
        <div style={{display:"flex",alignItems:"center",paddingTop:28,color:"var(--c-text-2)",fontSize:18,fontFamily:"var(--font-mono)"}}>x</div>
        <MatGrid data={B} type="B" label="MATRIX B" />
        <div style={{display:"flex",alignItems:"center",paddingTop:28,color:"var(--c-text-2)",fontSize:18,fontFamily:"var(--font-mono)"}}>=</div>
        <MatGrid data={C || Array.from({length:size},()=>Array(size).fill(""))} type="C" label="RESULT C" />
      </div>

      <StepLog entries={logs} />
    </section>
  );
}

// ── 4m. Dynamic Programming
function DPVisualizer({ title, subtitle, mode="fib" }) {
  const [n, setN] = useState(10);
  const [W, setW] = useState(10);
  const [dp, setDp] = useState([]);
  const [dpTable, setDpTable] = useState(null);
  const [active, setActive] = useState(-1);
  const [running, setRunning] = useState(false);
  const [logs, setLogs] = useState([]);
  const runRef = useRef(false);

  const runFib = async () => {
    const table=Array(n+1).fill(0);
    table[0]=0; if(n>0) table[1]=1;
    setDp([...table]); setActive(0);
    setLogs(l=>[...l,`dp[0]=0, dp[1]=1`]);
    for (let i=2;i<=n&&runRef.current;i++) {
      table[i]=table[i-1]+table[i-2];
      setDp([...table]); setActive(i);
      setLogs(l=>[...l,`dp[${i}] = dp[${i-1}](${table[i-1]}) + dp[${i-2}](${table[i-2]}) = ${table[i]}`]);
      await sleep(200);
    }
    setActive(-1); setLogs(l=>[...l,`Fib(${n}) = ${table[n]}`]);
  };

  const weights=[1,3,4,5], vals=[1,4,5,7];
  const runKnapsack = async () => {
    const nItems=weights.length, table=Array.from({length:nItems+1},()=>Array(W+1).fill(0));
    setDpTable(table.map(r=>[...r]));
    for (let i=1;i<=nItems&&runRef.current;i++) {
      for (let w=0;w<=W&&runRef.current;w++) {
        if (weights[i-1]<=w) table[i][w]=Math.max(table[i-1][w], table[i-1][w-weights[i-1]]+vals[i-1]);
        else table[i][w]=table[i-1][w];
        setDpTable(table.map(r=>[...r])); setActive(i*100+w);
        setLogs(l=>[...l,`dp[${i}][${w}] = ${table[i][w]}`]);
        await sleep(60);
      }
    }
    setActive(-1); setLogs(l=>[...l,`Max value = ${table[nItems][W]}`]);
  };

  const run = async () => {
    if (running){runRef.current=false;return;}
    runRef.current=true; setRunning(true); setDp([]); setDpTable(null); setLogs([]);
    if (mode==="fib"||mode==="tabulation"||mode==="memoization") await runFib();
    else await runKnapsack();
    runRef.current=false; setRunning(false);
  };

  return (
    <section className="panel" aria-labelledby={`${title}-heading`}>
      <header className="panel-header">
        <div>
          <h2 id={`${title}-heading`} className="panel-title">{title}</h2>
          <p className="panel-subtitle">{subtitle}</p>
        </div>
        <div className="panel-controls">
          {(mode==="fib"||mode==="tabulation"||mode==="memoization") && (
            <>
              <span style={{fontSize:11,color:"var(--c-text-3)"}}>n=</span>
              <input className="range" type="range" min="3" max="15" value={n} onChange={e=>setN(+e.target.value)} style={{width:80}} aria-label="n value" disabled={running}/>
              <span style={{fontSize:11,color:"var(--c-accent)",minWidth:20}}>{n}</span>
            </>
          )}
          {mode==="dp-knapsack" && (
            <>
              <span style={{fontSize:11,color:"var(--c-text-3)"}}>W=</span>
              <input className="range" type="range" min="5" max="15" value={W} onChange={e=>setW(+e.target.value)} style={{width:80}} aria-label="Knapsack capacity" disabled={running}/>
              <span style={{fontSize:11,color:"var(--c-accent)",minWidth:20}}>{W}</span>
            </>
          )}
          <Btn variant={running?"danger":"primary"} onClick={run}>{running?"Stop":"Run"}</Btn>
          <Btn variant="secondary" onClick={()=>{setDp([]);setDpTable(null);setLogs([]);setActive(-1);}} disabled={running}>Reset</Btn>
        </div>
      </header>

      {(mode==="fib"||mode==="tabulation"||mode==="memoization") && dp.length > 0 && (
        <div className="array-row" style={{flexWrap:"wrap",gap:"var(--sp-2)"}}>
          {dp.map((v,i) => (
            <div key={i} className="array-cell">
              <div className={`array-cell-box ${i===active?"comparing":v>0?"found":"idle"}`}>{v}</div>
              <div className="array-cell-idx">dp[{i}]</div>
            </div>
          ))}
        </div>
      )}

      {mode==="dp-knapsack" && dpTable && (
        <div style={{overflowX:"auto"}}>
          <table className="dp-table" aria-label="DP knapsack table">
            <thead>
              <tr>
                <td className="dp-cell header">i\W</td>
                {Array.from({length:W+1},(_,w)=><td key={w} className="dp-cell header">{w}</td>)}
              </tr>
            </thead>
            <tbody>
              {dpTable.map((row,i)=>(
                <tr key={i}>
                  <td className="dp-cell header">{i}</td>
                  {row.map((v,w)=>(
                    <td key={w} className={`dp-cell ${active===i*100+w?"active":v>0?"filled":""}`}>{v}</td>
                  ))}
                </tr>
              ))}
            </tbody>
          </table>
        </div>
      )}

      {dp.length===0&&!dpTable && <EmptyState icon="[ ]" title="No data" body="Press Run to start the DP computation." />}

      <StepLog entries={logs} />
    </section>
  );
}

// ── 4n. Floyd's Cycle Detection
function FloydVisualizer({ title, subtitle }) {
  const [list] = useState([1,3,5,7,9,5,3]); // has cycle back to index 2
  const [frames, setFrames] = useState([]);
  const [running, setRunning] = useState(false);
  const [speed, setSpeed] = useState(50);
  const runRef = useRef(false);

  const run = async () => {
    if (running){runRef.current=false;return;}
    runRef.current=true; setRunning(true); setFrames([]);
    const gen = floydCycleGen(list);
    const frms=[];
    for await (const f of gen) {
      if (!runRef.current) break;
      frms.push(f); setFrames([...frms]);
      await sleep(Math.max(50,400-speed*3));
    }
    runRef.current=false; setRunning(false);
  };

  const frame = frames[frames.length-1] || {slow:-1,fast:-1};
  const logs = frames.map(f=>f.log).filter(Boolean);

  return (
    <section className="panel" aria-labelledby={`${title}-heading`}>
      <header className="panel-header">
        <div>
          <h2 id={`${title}-heading`} className="panel-title">{title}</h2>
          <p className="panel-subtitle">{subtitle}</p>
        </div>
        <div className="panel-controls">
          <Btn variant={running?"danger":"primary"} onClick={run}>{running?"Stop":"Run"}</Btn>
          <Btn variant="secondary" onClick={()=>{setFrames([]);}} disabled={running}>Reset</Btn>
          <input className="range" type="range" min="1" max="100" value={speed} onChange={e=>setSpeed(+e.target.value)} style={{width:80}} aria-label="Speed"/>
        </div>
      </header>

      <div className="array-row" style={{gap:"var(--sp-1)"}}>
        {list.map((v,i) => (
          <div key={i} className="array-cell">
            <div className="array-cell-label">
              {frame.slow===i&&frame.fast===i?"S+F":frame.slow===i?"S":frame.fast===i?"F":""}
            </div>
            <div className={`array-cell-box ${frame.slow===i&&frame.fast===i?"found":frame.slow===i?"pointer-a":frame.fast===i?"pointer-b":"idle"}`}>{v}</div>
            <div className="array-cell-idx">{i}</div>
          </div>
        ))}
        <div style={{display:"flex",alignItems:"center",fontSize:12,color:"var(--c-text-3)",marginLeft:"var(--sp-2)"}}>
          cycle back to [2]
        </div>
      </div>

      <div className="legend">
        {[["Slow ptr","pointer-a"],["Fast ptr","pointer-b"],["Meet (cycle)","found"]].map(([l,cls])=>(
          <div key={l} className="legend-item">
            <div className={`legend-dot array-cell-box ${cls}`} style={{width:10,height:10}}/>
            <span>{l}</span>
          </div>
        ))}
      </div>

      <StepLog entries={logs} />
    </section>
  );
}

// ── 4o. Postfix Evaluator
function PostfixVisualizer({ title, subtitle }) {
  const [expr, setExpr] = useState("3 4 + 2 * 7 /");
  const [stack, setStack] = useState([]);
  const [tokens, setTokens] = useState([]);
  const [tokenIdx, setTokenIdx] = useState(-1);
  const [logs, setLogs] = useState([]);
  const [running, setRunning] = useState(false);
  const runRef = useRef(false);

  const run = async () => {
    if (running){runRef.current=false;return;}
    runRef.current=true; setRunning(true);
    const toks=expr.trim().split(/\s+/); setTokens(toks); setStack([]); setTokenIdx(-1); setLogs([]);
    const st=[];
    for (let i=0;i<toks.length&&runRef.current;i++) {
      const t=toks[i]; setTokenIdx(i);
      if (!isNaN(+t)) { st.push(+t); setStack([...st]); setLogs(l=>[...l,`Push ${t} → stack: [${st.join(", ")}]`]); }
      else {
        const b=st.pop(), a=st.pop();
        const res = t==="+"?a+b:t==="-"?a-b:t==="*"?a*b:t==="/"?+(a/b).toFixed(4):0;
        st.push(res); setStack([...st]);
        setLogs(l=>[...l,`${a} ${t} ${b} = ${res} → stack: [${st.join(", ")}]`]);
      }
      await sleep(500);
    }
    setTokenIdx(-1); setLogs(l=>[...l,`Result = ${st[0]}`]);
    runRef.current=false; setRunning(false);
  };

  return (
    <section className="panel" aria-labelledby={`${title}-heading`}>
      <header className="panel-header">
        <div>
          <h2 id={`${title}-heading`} className="panel-title">{title}</h2>
          <p className="panel-subtitle">{subtitle}</p>
        </div>
        <div className="panel-controls">
          <input className="input" type="text" value={expr} onChange={e=>setExpr(e.target.value)} placeholder="Postfix expression" style={{minWidth:200}} aria-label="Postfix expression" disabled={running}/>
          <Btn variant={running?"danger":"primary"} onClick={run}>{running?"Stop":"Evaluate"}</Btn>
          <Btn variant="secondary" onClick={()=>{setStack([]);setTokens([]);setTokenIdx(-1);setLogs([]);}} disabled={running}>Reset</Btn>
        </div>
      </header>

      <div style={{display:"grid",gridTemplateColumns:"1fr 1fr",gap:"var(--sp-6)"}}>
        <div>
          <div style={{fontSize:10,letterSpacing:2,color:"var(--c-text-3)",marginBottom:"var(--sp-3)"}}>TOKENS</div>
          <div className="array-row" style={{flexWrap:"wrap",gap:"var(--sp-2)"}}>
            {tokens.map((t,i) => (
              <div key={i} className="array-cell">
                <div className={`array-cell-box ${i===tokenIdx?"comparing":"idle"}`} style={{minWidth:40}}>{t}</div>
              </div>
            ))}
            {tokens.length===0 && <span style={{color:"var(--c-text-3)",fontSize:12}}>Awaiting evaluation...</span>}
          </div>
        </div>
        <div>
          <div style={{fontSize:10,letterSpacing:2,color:"var(--c-text-3)",marginBottom:"var(--sp-3)"}}>STACK (top)</div>
          <div style={{display:"flex",flexDirection:"column-reverse",gap:"var(--sp-1)",maxWidth:160}}>
            {stack.map((v,i) => (
              <div key={i} className={`array-cell-box ${i===stack.length-1?"found":"idle"}`} style={{width:"100%",paddingLeft:"var(--sp-4)",paddingRight:"var(--sp-4)"}}>
                {v}
              </div>
            ))}
            {stack.length===0 && <div className="array-cell-box idle" style={{color:"var(--c-text-3)"}}>empty</div>}
          </div>
        </div>
      </div>

      <StepLog entries={logs} />
    </section>
  );
}

// ── 4p. Vectorized Operations
function VectorizedVisualizer({ title, subtitle }) {
  const [size, setSize] = useState(8);
  const mkArr = (s) => Array.from({length:s},()=>Math.floor(Math.random()*20)+1);
  const [A, setA] = useState(()=>mkArr(8));
  const [B, setB] = useState(()=>mkArr(8));
  const [C, setC] = useState(null);
  const [op, setOp] = useState("add");
  const [activeIdx, setActiveIdx] = useState(-1);
  const [running, setRunning] = useState(false);
  const [logs, setLogs] = useState([]);
  const runRef = useRef(false);

  const regen = () => { const a=mkArr(size),b=mkArr(size); setA(a);setB(b);setC(null);setLogs([]); };

  const runOp = async () => {
    if (running){runRef.current=false;return;}
    runRef.current=true; setRunning(true);
    const result=Array(A.length).fill(0); setC([...result]); setLogs([]);
    for (let i=0;i<A.length&&runRef.current;i++) {
      setActiveIdx(i);
      const v = op==="add"?A[i]+B[i]:op==="mul"?A[i]*B[i]:op==="sub"?A[i]-B[i]:Math.max(A[i],B[i]);
      result[i]=v; setC([...result]);
      setLogs(l=>[...l,`C[${i}] = ${A[i]} ${op==="add"?"+":(op==="mul"?"*":(op==="sub"?"-":"max(·)"))} ${B[i]} = ${v}`]);
      await sleep(150);
    }
    setActiveIdx(-1); setLogs(l=>[...l,"Vectorized operation complete."]);
    runRef.current=false; setRunning(false);
  };

  return (
    <section className="panel" aria-labelledby={`${title}-heading`}>
      <header className="panel-header">
        <div>
          <h2 id={`${title}-heading`} className="panel-title">{title}</h2>
          <p className="panel-subtitle">{subtitle}</p>
        </div>
        <div className="panel-controls">
          <select className="select" value={op} onChange={e=>setOp(e.target.value)} aria-label="Operation">
            <option value="add">Element-wise Add</option>
            <option value="mul">Element-wise Multiply</option>
            <option value="sub">Element-wise Subtract</option>
            <option value="max">Element-wise Max</option>
          </select>
          <select className="select" value={size} onChange={e=>{setSize(+e.target.value);setC(null);}} aria-label="Vector size">
            {[4,6,8,10].map(s=><option key={s} value={s}>n={s}</option>)}
          </select>
          <Btn variant="secondary" onClick={regen} disabled={running}>Randomize</Btn>
          <Btn variant={running?"danger":"primary"} onClick={runOp}>{running?"Stop":"Run"}</Btn>
        </div>
      </header>

      {[["VECTOR A",A],["VECTOR B",B],["RESULT C",C||Array(A.length).fill("")]].map(([label,arr])=>(
        <div key={label} style={{marginBottom:"var(--sp-4)"}}>
          <div style={{fontSize:10,letterSpacing:2,color:"var(--c-text-3)",marginBottom:"var(--sp-2)"}}>{label}</div>
          <div className="array-row">
            {arr.map((v,i)=>(
              <div key={i} className="array-cell">
                <div className={`array-cell-box ${label==="RESULT C"&&i===activeIdx?"comparing":label==="RESULT C"&&v!==""?"found":label!=="RESULT C"&&i===activeIdx?"active":"idle"}`}>{v}</div>
                <div className="array-cell-idx">{i}</div>
              </div>
            ))}
          </div>
        </div>
      ))}

      <StepLog entries={logs} />
    </section>
  );
}

// ─── 5. VISUALIZER ROUTER ──────────────────────────────────────────────────────

function useArrayAlgo(algoFn, initialArr, target) {
  const [array] = useState(initialArr || Array.from({length:20},()=>Math.floor(Math.random()*95)+5));
  const [frames, setFrames] = useState([{arr:[...array]}]);
  const [running, setRunning] = useState(false);
  const [speed, setSpeed] = useState(50);
  const runRef = useRef(false);

  const run = async () => {
    if (running){runRef.current=false;return;}
    runRef.current=true; setRunning(true);
    setFrames([{arr:[...array]}]);
    const frms=[{arr:[...array]}];
    const gen = algoFn(array, target);
    for await (const f of gen) {
      if (!runRef.current) break;
      frms.push(f); setFrames([...frms]);
      await sleep(Math.max(5,300-speed*2.9));
    }
    runRef.current=false; setRunning(false);
  };

  const reset = () => { setFrames([{arr:[...array]}]); };
  return { frames, run, reset, running, speed, setSpeed };
}

function SortPanel({ algoFn, title, subtitle }) {
  const { frames, run, reset, running, speed, setSpeed } = useArrayAlgo(algoFn);
  return <ArrayVisualizer frames={frames} onRun={run} onReset={reset} running={running} speed={speed} onSpeedChange={setSpeed} title={title} subtitle={subtitle} showBars={true} />;
}

function SearchPanel({ algoFn, sorted, title, subtitle, twoPtr=false, window=false }) {
  const base = sorted
    ? [3,7,12,18,24,31,37,45,52,58,63,71,77,84,90,96]
    : Array.from({length:16},()=>Math.floor(Math.random()*90)+5);
  const target = twoPtr ? 91 : window ? 4 : (sorted ? 45 : base[Math.floor(Math.random()*base.length)]);
  const { frames, run, reset, running, speed, setSpeed } = useArrayAlgo(algoFn, base, target);

  const controls = (
    <div className="info-bar" style={{marginBottom:0,marginRight:"var(--sp-2)"}}>
      <StatBar label="Target" value={twoPtr ? "sum=91" : window ? "k=4" : String(target)} />
    </div>
  );

  return <ArrayVisualizer frames={frames} onRun={run} onReset={reset} running={running} speed={speed} onSpeedChange={setSpeed} title={title} subtitle={subtitle} controls={controls} showBars={false} />;
}

function VisRouter({ id }) {
  const cplx = COMPLEXITY[id] || { time: "—", space: "—" };

  const Complexity = () => (
    <div className="info-bar">
      <StatBar label="Time" value={cplx.time} />
      <StatBar label="Space" value={cplx.space} />
    </div>
  );

  const panels = {
    "arrays": () => (
      <div>
        <Complexity />
        <SearchPanel algoFn={linearSearchGen} sorted={false} title="Arrays" subtitle="Random-access, O(1) index lookup. Search = linear scan." />
      </div>
    ),
    "strings": () => (
      <div>
        <Complexity />
        <SearchPanel algoFn={linearSearchGen} sorted={false} title="Strings" subtitle="Character array. Visualized as linear scan over characters." />
      </div>
    ),
    "singly-ll": () => (<div><Complexity /><LinkedListVisualizer title="Singly Linked List" subtitle="Each node holds a value and a pointer to the next node." doubly={false} /></div>),
    "doubly-ll": () => (<div><Complexity /><LinkedListVisualizer title="Doubly Linked List" subtitle="Each node holds forward and backward pointers." doubly={true} /></div>),
    "stacks": () => (<div><Complexity /><StackQueueVisualizer title="Stacks" subtitle="LIFO: Last-In, First-Out. Push and Pop from the top." mode="stack" /></div>),
    "queues": () => (<div><Complexity /><StackQueueVisualizer title="Queues" subtitle="FIFO: First-In, First-Out. Enqueue at rear, dequeue from front." mode="queue" /></div>),
    "circular-queue": () => (<div><Complexity /><CircularQueueVisualizer title="Circular Queue" subtitle="Fixed-size ring buffer — front and rear wrap around." /></div>),
    "hash-table": () => (<div><Complexity /><HashVisualizer title="Hash Table" subtitle="Key → hash function → bucket index. Chaining for collisions." mode="chaining" /></div>),
    "hash-map": () => (<div><Complexity /><HashVisualizer title="Hash Map" subtitle="Key-value store backed by a hash table with chaining." mode="chaining" /></div>),
    "collision": () => (<div><Complexity /><HashVisualizer title="Collision Resolution" subtitle="Separate chaining: multiple keys in the same slot form a chain." mode="chaining" /></div>),
    "binary-tree": () => (<div><Complexity /><TreeVisualizer title="Binary Tree" subtitle="Each node has at most two children (left and right)." mode="bst" /></div>),
    "bst": () => (<div><Complexity /><TreeVisualizer title="Binary Search Tree" subtitle="Left < Root < Right. O(log n) average insert/search." mode="bst" /></div>),
    "avl": () => (<div><Complexity /><TreeVisualizer title="AVL Tree" subtitle="Self-balancing BST. Heights of subtrees differ by at most 1." mode="avl" /></div>),
    "heap-struct": () => (<div><Complexity /><HeapVisualizer title="Heap" subtitle="Complete binary tree satisfying the heap property (max-heap shown)." /></div>),
    "priority-queue": () => (<div><Complexity /><HeapVisualizer title="Priority Queue" subtitle="Abstract data type backed by a heap. Extract-max in O(log n)." /></div>),
    "trie": () => (<div><Complexity /><TrieVisualizer title="Trie (Prefix Tree)" subtitle="Tree for string storage. Each edge = one character." /></div>),
    "btree": () => (<div><Complexity /><TreeVisualizer title="B-Tree" subtitle="Balanced multi-way tree used in databases and file systems." mode="bst" /></div>),
    "directed": () => (<div><Complexity /><GraphVisualizer title="Directed Graph" subtitle="Edges have direction. Supports BFS, DFS, topological sort." mode="directed" /></div>),
    "undirected": () => (<div><Complexity /><GraphVisualizer title="Undirected Graph" subtitle="Edges are bidirectional. Supports BFS and DFS traversal." mode="undirected" /></div>),
    "weighted": () => (<div><Complexity /><GraphVisualizer title="Weighted Graph" subtitle="Edges have weights. Supports Dijkstra, Kruskal, Prim." mode="weighted" /></div>),
    "union-find": () => (<div><Complexity /><UnionFindVisualizer title="Disjoint Sets (Union-Find)" subtitle="Track connected components. Union by rank + path compression." /></div>),
    "linear-search": () => (<div><Complexity /><SearchPanel algoFn={linearSearchGen} sorted={false} title="Linear Search" subtitle="Scan each element left-to-right until target found. O(n)." /></div>),
    "binary-search": () => (<div><Complexity /><SearchPanel algoFn={binarySearchGen} sorted={true} title="Binary Search" subtitle="Halve the search space each step. Requires sorted array. O(log n)." /></div>),
    "floyd-cycle": () => (<div><Complexity /><FloydVisualizer title="Floyd's Tortoise & Hare" subtitle="Two pointers at different speeds detect cycles in O(n), O(1) space." /></div>),
    "dfs": () => (<div><Complexity /><GraphVisualizer title="Depth-First Search" subtitle="Explore as deep as possible before backtracking. Uses a stack." mode="undirected" /></div>),
    "bfs": () => (<div><Complexity /><GraphVisualizer title="Breadth-First Search" subtitle="Explore layer by layer. Uses a queue. Finds shortest path." mode="undirected" /></div>),
    "tree-traversal": () => (<div><Complexity /><TreeVisualizer title="Tree Traversals" subtitle="In-order, pre-order, post-order. Each visits all nodes exactly once." mode="bst" /></div>),
    "postfix": () => (<div><Complexity /><PostfixVisualizer title="Postfix / Prefix Evaluation" subtitle="Evaluate RPN expressions using a stack. No parentheses needed." /></div>),
    "merge-sort": () => (<div><Complexity /><SortPanel algoFn={mergeSortGen} title="Merge Sort" subtitle="Divide array in half, sort each half, merge. Stable. O(n log n)." /></div>),
    "quick-sort": () => (<div><Complexity /><SortPanel algoFn={quickSortGen} title="Quick Sort" subtitle="Partition around pivot. O(n log n) average, O(n^2) worst case." /></div>),
    "two-pointer": () => (<div><Complexity /><SearchPanel algoFn={twoPointerGen} sorted={true} title="Two-Pointer Technique" subtitle="Left and right pointers converge to find pair summing to target." twoPtr={true} /></div>),
    "sliding-window": () => (<div><Complexity /><SearchPanel algoFn={slidingWindowGen} sorted={false} title="Sliding Window" subtitle="Fixed-size window slides over array to find max subarray sum." window={true} /></div>),
    "dijkstra": () => (<div><Complexity /><PathfindingVisualizer title="Dijkstra's Algorithm" subtitle="Shortest path via priority queue. No negative edges." /></div>),
    "astar": () => (<div><Complexity /><PathfindingVisualizer title="A* Search" subtitle="Dijkstra + heuristic. Faster in practice for spatial graphs." /></div>),
    "kruskal": () => (<div><Complexity /><GraphVisualizer title="Kruskal's Algorithm" subtitle="Sort edges by weight, add if no cycle. Produces MST." mode="weighted" /></div>),
    "prim": () => (<div><Complexity /><GraphVisualizer title="Prim's Algorithm" subtitle="Grow MST one edge at a time, always taking the cheapest reachable." mode="weighted" /></div>),
    "topological": () => (<div><Complexity /><GraphVisualizer title="Topological Sort" subtitle="Linear ordering of vertices in a DAG. Kahn's BFS-based algorithm." mode="directed" /></div>),
    "dp-fib": () => (<div><Complexity /><DPVisualizer title="Dynamic Programming (Fibonacci)" subtitle="Bottom-up DP. Compute fib(n) by filling a table left-to-right." mode="fib" /></div>),
    "dp-knapsack": () => (<div><Complexity /><DPVisualizer title="0/1 Knapsack" subtitle="Fill DP table: dp[i][w] = max value using i items with capacity w." mode="dp-knapsack" /></div>),
    "memoization": () => (<div><Complexity /><DPVisualizer title="Memoization (Top-Down DP)" subtitle="Cache recursive calls. Same recurrence as fib, different order." mode="memoization" /></div>),
    "tabulation": () => (<div><Complexity /><DPVisualizer title="Tabulation (Bottom-Up DP)" subtitle="Build solution iteratively from smallest subproblems." mode="tabulation" /></div>),
    "matrix-mult": () => (<div><Complexity /><MatrixVisualizer title="Matrix Multiplication" subtitle="O(n^3) naive. Element C[i][j] = dot product of row i and col j." /></div>),
    "heapify": () => (<div><Complexity /><HeapVisualizer title="Heapify" subtitle="Convert arbitrary array into a valid max-heap in O(n)." /></div>),
    "extract-minmax": () => (<div><Complexity /><HeapVisualizer title="Extract Min/Max" subtitle="Remove root (max), replace with last element, sink down. O(log n)." /></div>),
    "path-compress": () => (<div><Complexity /><UnionFindVisualizer title="Path Compression" subtitle="During Find, point nodes directly to root. Amortized O(alpha n)." /></div>),
    "vectorized": () => (<div><Complexity /><VectorizedVisualizer title="Vectorized Operations" subtitle="Element-wise ops on arrays. SIMD exploits CPU parallelism." /></div>),
  };

  const render = panels[id];
  if (!render) {
    return (
      <div className="panel">
        <EmptyState icon="?" title="Not implemented" body={`Visualizer for "${id}" is not yet available.`} />
      </div>
    );
  }
  return render();
}

// ─── 6. SIDEBAR ───────────────────────────────────────────────────────────────

function Sidebar({ activeId, onSelect }) {
  const [open, setOpen] = useState(() => {
    const s = {};
    CATEGORIES.forEach(c => { s[c.id] = true; });
    return s;
  });

  const toggle = (id) => setOpen(o => ({...o, [id]: !o[id]}));

  return (
    <nav className="synapse-sidebar" aria-label="Algorithm categories">
      {CATEGORIES.map(cat => (
        <div key={cat.id} className="sidebar-category">
          <button
            className="sidebar-cat-header"
            onClick={() => toggle(cat.id)}
            aria-expanded={open[cat.id]}
            aria-controls={`cat-${cat.id}`}
          >
            <div style={{display:"flex",alignItems:"center",gap:"var(--sp-2)"}}>
              <div className="sidebar-cat-indicator" style={{background:cat.color}} aria-hidden="true" />
              {cat.label}
            </div>
            <span className={`sidebar-cat-chevron${open[cat.id]?" open":""}`} aria-hidden="true">›</span>
          </button>

          {open[cat.id] && (
            <div id={`cat-${cat.id}`} className="sidebar-items" role="list">
              {cat.items.map(item => (
                <button
                  key={item.id}
                  role="listitem"
                  className={`sidebar-item${activeId===item.id?" active":""}`}
                  onClick={() => onSelect(item.id)}
                  aria-current={activeId===item.id?"page":undefined}
                >
                  {item.label}
                </button>
              ))}
            </div>
          )}
        </div>
      ))}
    </nav>
  );
}

// ─── 7. SHELL ─────────────────────────────────────────────────────────────────

export default function Synapse() {
  const [activeId, setActiveId] = useState("merge-sort");
  const activeItem = CATEGORIES.flatMap(c=>c.items).find(i=>i.id===activeId);
  const activeCategory = CATEGORIES.find(c=>c.items.some(i=>i.id===activeId));

  return (
    <div className="synapse-shell">
      <header className="synapse-header" role="banner">
        <div className="header-brand">
          <span className="header-wordmark">SYNAPSE</span>
          <span className="header-tagline">Algorithm Visualizer</span>
        </div>
        <div className="header-meta">
          {activeCategory && (
            <span style={{color:"var(--c-text-2)"}}>
              <span style={{color:activeCategory.color}}>{activeCategory.label}</span>
              {" / "}{activeItem?.label}
            </span>
          )}
        </div>
      </header>

      <Sidebar activeId={activeId} onSelect={setActiveId} />

      <main className="synapse-main" id="main-content" tabIndex={-1}>
        <VisRouter key={activeId} id={activeId} />
      </main>
    </div>
  );
}
