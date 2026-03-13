# SYNAPSE v2
SYNAPSE v2 is a zero-dependency, interactive Algorithm and Data Structure Visualizer built with React. It uses JavaScript generator functions and SVG to animate and explain computer science concepts in real-time.

Features
Interactive Animations: Step-by-step visualizations for algorithms and data structures.

Live Execution Log: Real-time text output explaining logical comparisons at each step.

Complexity References: Built-in Big-O time and space complexity cheat sheets.

Execution Controls: Play, Stop, Reset, and Animation Speed adjustments.

Self-Contained: Pure CSS custom properties injected via JavaScript with no external dependencies.

Supported Concepts
Linear & Hash Structures: Arrays, Linked Lists, Stacks, Queues, Hash Maps.

Trees & Graphs: BST, AVL, B-Trees, Heaps, Tries, Disjoint Sets.

Search & Traversal: Linear/Binary Search, BFS, DFS, Tree Traversals.

Sorting & Techniques: Merge Sort, Quick Sort, Two-Pointer, Sliding Window.

Advanced Algorithms: Pathfinding (Dijkstra, A*), Spanning Trees (Kruskal, Prim), Dynamic Programming (Fibonacci, Knapsack), Topological Sort.

Numeric: Matrix Multiplication, Vectorized Operations.

Tech Stack
Framework: React (Functional Components, Hooks).

Execution: JavaScript async function* (generators) to control algorithmic steps without blocking the main thread.

Rendering: DOM/CSS for arrays; dynamic <svg> for Trees, Graphs, and Linked Lists.
