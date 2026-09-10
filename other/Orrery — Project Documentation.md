
## 1. What Orrery Is

Orrery is a writing and note-taking app. You write in it like you would in any markdown editor, but it has one extra ability: at any point in a piece of writing, you can branch off an alternate version of what happens next, see that branch as a simple diagram, and decide whether to keep it, discard it, or bring parts of it back into the main document.

It's built for anyone writing something that unfolds over time and has moments where "what if this went differently" is a real, useful question to ask, not just fiction writers, though that's the original use case.

## 2. The Core Idea

The idea started from a specific moment: writing a story or script, and at some point wondering "what if this event had gone differently right here?" Instead of just imagining it and moving on, or copying the whole document to try a different path, Orrery lets you mark that moment as a checkpoint, generate an alternate branch from it with AI, and see the result as a small diagram, checkpoint and branch as nodes, connected by a line.

The name comes from a mechanical model of the solar system, the idea being a tool that shows how one thing relates to and branches from another, at a glance, the same way an orrery shows how planets relate to each other. That's the only place the "planets and orbits" idea shows up. It's not a design principle for the graph itself, the graph is nodes and edges, not physics.

## 3. Who It's For

The core case is creative writing: stories, scripts, screenplays, any narrative with decision points worth exploring.

The same mechanic (a checkpoint, a branch, an AI-generated alternate, a way to review and merge it back) applies naturally to other kinds of writing that also unfold as a sequence of events or decisions, without needing new features:

- Personal decision journaling ("what if I'd taken the other option at this point")
- Architecture or design decision records ("what if we'd chosen X instead of Y here")
- Research notes ("what if this variable had come out differently")

This list is deliberately not broad. The mechanic only makes sense for content that has a real sequence and real decision points. It's not being positioned as a general-purpose note app for everything.

## 4. How It Works (Workflow)

1. **Capture** — Write normally in the block editor. Content is organized into checkpoints as you go.
2. **Fork** — At any checkpoint, trigger an AI-generated alternate branch, "what if this went differently starting here."
3. **Visualize** — Open the canvas view for that note to see its checkpoint tree: the main version and every branch, laid out top to bottom as a simple diagram.
4. **Review** — Before a generated branch is added permanently, review it in a side-by-side view and accept, edit, or reject it. This is the same review step whether the AI generated a full branch or just edited a paragraph, one consistent way of checking AI output before it sticks.
5. **Merge** — Bring an accepted branch's content back into the main document, or leave it alongside the main version as a reference.

The canvas view is scoped to one note at a time. It shows that note's checkpoints and branches, not a graph of every note in the whole app. Moving between different notes is a plain list or folder view in the sidebar, like a normal file browser, not a graph.

## 5. Design Direction

The overall goal is calm and easy to understand, not visually busy or overwhelming. Specific references and what's being taken from each:

- **Obsidian** — the base layout: a collapsible sidebar, a main editing area, minimal top chrome, nothing decorative competing with the writing.
- **Craft** — the block and sidebar structure (organizing content into clear block types, clean navigation), but not Craft's heavier visual styling, no soft shadows, gradients, or theme customization options. Structure without the visual weight.
- **Kinopio** — small nuances of flat, two-dimensional design: flat fills, thin outlines instead of shadows for depth, simple connector lines between nodes. Just the visual flatness, not Kinopio's sketchy or playful feel.
- **Sudowrite's Canvas** — the reference for what the checkpoint/branch diagram should feel like to use: a visual board tied directly to the document, where triggering a branch from a point in the writing is the main interaction.

Color use should stay limited, a small fixed set of colors tied to block or checkpoint type, not decorative variety. Motion (via Framer Motion) should be subtle, pane transitions and hover states, not bouncy or attention-grabbing.

## 6. Data Model (conceptual)

Each note's checkpoints and branches form a tree: one starting point, any number of forks, each fork able to fork again. This is the same shape as version control history (like Git commits), which is why the technical choices below lean on tools built for exactly that shape rather than tools built for open-ended, everything-connects-to-everything graphs.

## 7. Tech Stack

**Frontend framework: Next.js (App Router)** Kept for routing and any public-facing pages. The actual editor and canvas views are rendered fully on the client, since none of that data lives on a server that could render it ahead of time.

**Styling: Tailwind CSS + Radix UI** Gives full control over the flat, minimal look without extra design-system weight.

**Motion: Framer Motion** Used for the subtle transitions and hover states described above.

**State management: Zustand** Lightweight, no more ceremony than the app needs.

**Text editor: TipTap** Handles the markdown-style block editor, including formatting, checklists, and tables. This is the same editor foundation used by most modern block-based writing tools.

**Checkpoint/branch canvas: React Flow (@xyflow/react) with automatic tree layout (dagre or elkjs)** Since the checkpoint/branch structure is a tree, not a physics simulation, a library built for drawing nodes, edges, and custom node types with a proper hierarchical layout is a better fit than a force-directed physics engine. Also handles the drag-and-connect interactions directly.

**Local storage: Dexie.js (IndexedDB)** Keeps the app fast and usable offline. Stores everything locally first.

**Sync (local-first with cloud backup): Yjs** Handles merging changes made on different devices, including when someone forks a branch offline on one device and edits something on another. Specifically:

- `y-indexeddb` to persist the local copy of each note's checkpoint/branch structure
- A sync server, either self-hosted (`y-websocket`) or a hosted service (Liveblocks or y-sweet), to move changes between devices This avoids writing custom conflict-resolution logic by hand for a tree structure that can be edited from multiple places.

**Backend database: Postgres, via Supabase** Checkpoints and branches are naturally stored as rows in a table that reference their own parent row, and queried with recursive queries to reconstruct a branch's full history. This is a good fit for a relational database and a poor fit for a document database.

**Authentication: Firebase Auth (existing) or consolidated into Supabase Auth** Not yet decided, see below.

**AI: OpenRouter** Used to call different AI models (Claude, GPT, Nemotron, etc.) for generating alternate branches, without being locked into a single provider.

**Hosting: Vercel (frontend), Supabase (database), a small always-on service such as Fly.io or Render (sync server, if self-hosted)**

## 8. Not Yet Decided

- Whether authentication stays on Firebase or moves to Supabase Auth alongside the database, to reduce the number of separate services.
- Whether to self-host the Yjs sync server or use a hosted service like Liveblocks or y-sweet.
- The exact shape of cross-note navigation in the sidebar (plain list, folders, tags, or some combination).