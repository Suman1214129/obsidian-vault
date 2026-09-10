### 🌌 1. What is Orrery?

**Orrery** brings that same gravitational elegance to the universe of human thought. It is a **local-first personal knowledge management (PKM) and cognitive cartography platform**. Unlike traditional note apps (static folders of text) or generic link graphs (unweighted spiderwebs), Orrery functions as an **active thinking laboratory** that models ideas as dynamic celestial bodies with gravitational pull, semantic checkpoints, and speculative counterfactual branches ("What-If" simulations).

---

### 🧭 2. Core Philosophy & Guiding Principles

1. **Gravitational Cognition, Not Hierarchical Silos**: Thoughts don't naturally live in rigid folder trees. Orrery applies gravitational topology where concepts attract, repel, and orbit based on semantic relationship strength, recency, and causal dependency.
2. **Speculative & Counterfactual Thinking**: Most note tools only record *what happened*. Orrery lets you explore *what could happen*—forking any decision, thesis, or plot point to simulate consequences, weigh trade-offs, and merge insights back into the mainline document.
3. **Local-First Sovereignty & Absolute Privacy**: Data is stored client-side in **IndexedDB (Dexie.js)**. Reads and writes have zero latency, work 100% offline, and preserve user ownership with zero vendor lock-in.
4. **Ambient, Non-Intrusive Intelligence**: AI acts as an analytical lens (structural checkpoint extraction, counterargument suggestions, consequence modeling) accompanied by visual **Diff Reviews** before any change is accepted.
5. **Kinetic, Tactile Focus**: Minimalist typography (TipTap) meets organic motion (D3.js physics & Framer Motion), curated themes (Light, Dark, Sepia), and bespoke retro pixel loading animations.

---

### 🎯 3. Primary Use Cases

* **Creative Writers & Worldbuilders**: Multi-timeline plotting, character choice consequence forecasting, and narrative timeline graphs.
* **Researchers & Academics**: Literature synthesis, hypothesis-to-finding mapping, and vertical debate graphs (thesis vs. counterargument).
* **Product Strategists & Founders**: Strategic decision trees, trade-off evaluations (pros/cons), and roadmapping.
* **Software Architects**: RFC drafting, architectural decision records (ADRs), and failure-mode simulations.
* **Reflective Thinkers & Journalers**: Habit tracking, emotional state reflections, and serendipitous thought rediscovery.

---

### ⚡ 4. Core Features

* **TipTap Prose Engine**: Rich markdown editor with syntax-highlighted code blocks (`lowlight`), task lists, tables, and bi-directional `[[wiki-linking]]`.
* **Intelligent Knowledge Graph (D3.js)**: 2D force-directed physics engine supporting 10 note modalities (*Story, Research, Argument, Process, Decision, Concept, Meeting, Technical, Journal, Brainstorm*) across multiple layouts (*Network Web, Timeline, Debate, Tree, Action Board*).
* **Universal Checkpoints & "What-If" Branching**: Deconstructs linear text into atomic checkpoints connected by causal, temporal, supportive, or contradictory relationships. Allows speculative branching with automatic consequence generation.
* **Diff Review Overlay**: Color-coded side-by-side modal to inspect and selectively accept AI proposals or branch merges.
* **Local-First Database (Dexie.js)**: Full-featured IndexedDB schema for notes, folders, canvases, narrative graphs, and AI history with one-click JSON backup export/import.
* **Firebase Authentication (v12)**: Google OAuth and Email/Password sign-in, secure sessions, password recovery, and Danger Zone tools (nuclear local data wipe, account deletion).
* **Spatial Command Palette (`⌘K`)**: Rapid keyboard-first navigation across all notes, tags, and actions.

---

### 🛠️ 5. Current Tech Stack

| Layer | Technologies |
| :--- | :--- |
| **Framework & Core** | **Next.js 16.1.1** (App Router, Turbopack), **React 19.2.3**, **TypeScript 5** |
| **Styling & Motion** | **Tailwind CSS v4**, **Framer Motion 12**, **Radix UI**, **Lucide Icons** |
| **State Management** | **Zustand 5** (`notesStore`, `uiStore`, `graphStore`, `authStore`) |
| **Prose Editor** | **TipTap v3**, ProseMirror, `lowlight` syntax highlighting |
| **Visualization** | **D3.js v7** (`d3-force`), **@xyflow/react** (React Flow canvas) |
| **Storage & Database**| **Dexie.js v4** (IndexedDB client-side storage) |
| **Auth & AI** | **Firebase SDK v12**, **OpenRouter API** (Nemotron, Claude, GPT) |

---

### 🚀 6. Recommended Tech Stack Evolution

1. **Hybrid CRDT Sync**: Integrate **ElectricSQL** or **Yjs / Supabase** to provide cross-device sync while keeping local-first zero-latency guarantees.
2. **Local Vector Embeddings**: In-browser vector search with **Orama** or **Transformers.js** (`all-MiniLM-L6-v2`) for offline semantic search.
3. **Web Worker Offloading**: Move D3 physics simulations to a Web Worker via `comlink` to maintain 120 FPS on vaults with 10,000+ nodes.
4. **End-to-End Encryption (E2EE)**: Client-side AES-GCM encryption before syncing to any cloud provider.
5. **Automated Testing Suite**: **Vitest** for store logic/heuristics and **Playwright** for end-to-end browser verification.

---

### 🔄 7. The 5-Stage Orrery Workflow

```
[ 1. CAPTURE ] ──► [ 2. CONNECT ] ──► [ 3. ANALYZE ] ──► [ 4. SPECULATE ] ──► [ 5. SYNTHESIZE ]
 Freeform Prose     [[Wiki-Links]]     Checkpoint Graph    "What-If" Scenarios   Diff & Mainline Merge
```

1. **Capture**: Rapid draft via `⌘K` or the TipTap editor with markdown shortcuts.
2. **Connect**: Link related concepts using `[[` notation and thematic tags (`#tag`).
3. **Analyze**: Switch to the **Graph View** to let Orrery automatically map atomic checkpoints, dependencies, and debate threads.
4. **Speculate**: Trigger **"What-If" Branching** on key decision or plot checkpoints to forecast downstream consequences and trade-offs.
5. **Synthesize**: Review generated branches with the **Diff Review Overlay** and commit selected insights into the mainline document.

---

### 📂 File References
* Full Guide: [ORRERY.md](file:///c:/Users/Suman%20Yadav/Desktop/ORRERY/ORRERY.md)
* Repository Readme: [README.md](file:///c:/Users/Suman%20Yadav/Desktop/ORRERY/README.md)