# Orrery Migration Prompt (for a coding agent)

Give this whole document to the agent as its task brief. It assumes the agent has direct access to the existing Orrery repository. It does not assume the agent has any other context about the project, so read it in full before starting.

Also give the agent `orrery-skills.md` alongside this file. That file holds the rules that apply to every phase below and to any future work on this project, not just this migration. Keep referring back to it.

---

## 0. What you're working on, in plain terms

Orrery is a writing app. Someone writes a story, script, decision log, or research note in a block editor. At any point in the writing, they can mark a checkpoint and ask "what if this went differently from here," and the app generates an alternate branch with AI. Branches show up as a simple diagram (nodes and lines) scoped to that one document, not a graph of the whole app. The person can review a generated branch, accept or reject it, and merge it back into the main document if they want.

The existing codebase was built with a different framing (local-only storage, a physics-based graph for visualization) and a rougher UI direction. This migration moves it to the decisions below without rewriting the app from scratch.

## 1. Before touching anything: audit the existing codebase

Do not assume any of the following, confirm it by reading the actual code:

- Where and how the current Dexie/IndexedDB schema is defined, and which parts of the data model it covers (notes, checkpoints, branches, AI history, etc.)
- Every place `d3-force` (or any D3 physics simulation) is used. Confirm whether it's used only for the checkpoint/branch graph or also somewhere else in the app.
- The current Firebase Auth setup: how sessions are created, where auth state is read from in the UI, and whether anything else in the codebase (rules, storage permissions) depends on Firebase specifically.
- The current OpenRouter integration: where API calls are made, what prompt structure is used for AI generation, and whether there's already any kind of review/accept-reject UI for AI output.
- The current sidebar and navigation structure.
- The current TipTap editor setup and whatever checkpoint-marking mechanism, if any, already exists.

Write a short inventory (a few paragraphs, not a full report) covering what you found before starting Phase 2. If anything below assumes something that turns out to be false once you've read the code, flag it instead of working around it silently.

## 2. Target stack (what you're migrating to)

Keep as-is: Next.js (App Router), TypeScript, Tailwind CSS, Framer Motion, Radix UI, Lucide Icons, Zustand, TipTap, OpenRouter.

Replace or add:

- **Checkpoint/branch canvas**: `@xyflow/react` (React Flow) with `dagre` or `elkjs` for automatic tree layout, replacing `d3-force` for this specific view. See `orrery-skills.md` for why (short version: the checkpoint/branch structure is a tree, not a physics simulation).
- **Local-first sync layer**: Yjs, specifically `y-indexeddb` for local persistence of each note's checkpoint/branch structure, plus a sync transport (see Phase 5, this is a flagged decision, not a default to apply silently).
- **Backend database**: Postgres via Supabase, for storing checkpoints and branches as rows that reference their own parent row (self-referencing table), queried with recursive queries to reconstruct a branch tree.
- **Auth**: default to leaving Firebase Auth as-is. Do not migrate auth to Supabase unless Phase 6 explicitly calls for it. This is a deliberate choice to reduce migration risk, not an oversight.

## 3. Phase 1 — Data model foundation

Do this before any UI changes.

1. Design the checkpoint table: an id, a reference to the note it belongs to, a reference to its parent checkpoint (nullable, null means it's the root of that note), its content, a flag or grouping value marking whether it's on the mainline or a branch, and a created-at timestamp.
2. Write the Postgres migration for this via Supabase.
3. Write and test a recursive query that returns a full checkpoint tree for a given note, ordered so it can be laid out top to bottom without extra client-side sorting.
4. Define how this maps onto a Yjs document per note (one Y.Doc per note is the simplest starting point, holding that note's checkpoint tree as a Yjs data structure).
5. Do not wire this into the UI yet. This phase is data-model only, confirm it with a script or test, not by clicking through the app.

## 4. Phase 2 — Replace the checkpoint canvas

1. Remove `d3-force` usage for the checkpoint/branch view specifically. If your Phase 0 audit found it used elsewhere too, leave those other usages alone for now and flag them instead of removing.
2. Build the canvas as a React Flow component that takes a single note's id as input and renders only that note's checkpoint tree, nothing else. Confirm this against `orrery-skills.md`, section on scope: this is never a cross-note graph.
3. Use `dagre` or `elkjs` for layout, top to bottom, mainline checkpoints in a straight line and branches forking visibly off to the side.
4. Custom node component per checkpoint: flat fill, thin border instead of a drop shadow, no gradient. See the design rules in `orrery-skills.md` before styling this.

## 5. Phase 3 — Fork and review flow

1. Add a "fork from here" action on any checkpoint (button or context menu on the node, and/or an action available from the text cursor position in the editor if that's where checkpoints are marked).
2. Wire this to the existing OpenRouter integration to generate alternate content starting from that checkpoint.
3. The generated content becomes a new checkpoint row with its parent set to the origin checkpoint, marked as a branch (not mainline).
4. Before this is written permanently, it must go through a review step, reuse or adapt whatever accept/reject/edit UI already exists for AI-generated text edits (per your Phase 0 audit) rather than building a second, separate review UI. If nothing like that already exists, build one review pattern and use it for both text edits and generated branches going forward.
5. Add a "merge" action that takes an accepted branch's content and applies it to the mainline, and confirm what "leave as a parallel branch instead of merging" looks like too, both should be real options, not just merge-or-discard.

## 6. Phase 4 — Sync

This phase has a decision that needs a person to make, do not resolve it yourself and move on.

**Flag this back to whoever is reviewing your work**: should the sync transport be self-hosted (`y-websocket` on a small always-on service like Fly.io or Render) or a hosted service (Liveblocks or y-sweet)? Self-hosted means more control and no new vendor, hosted means less infrastructure to maintain. State this as an open question in your output rather than picking one and proceeding.

Once that's answered:

1. Wire `y-indexeddb` (from Phase 1) to the chosen sync transport.
2. Confirm the offline scenario works: create a fork on one device while offline, edit something else on another device, reconnect both, confirm both changes are present and nothing was silently overwritten.
3. Confirm the Postgres database and the Yjs documents don't drift out of sync with each other, decide (and document) which one is the source of truth for reads versus which one sync flows through.

## 7. Phase 5 — UI and design pass

Apply this after the functional pieces above work, not before. Follow `orrery-skills.md`'s design rules in full for this phase, the short version:

1. Sidebar: a plain collapsible list or folder tree of notes, Obsidian-style. No tag-based or cross-note graph anywhere in navigation.
2. Block organization inside a note follows Craft's structural pattern (clear block types, clean grouping) without Craft's heavier visual styling, no soft shadows, no theme customization panel.
3. Apply the flat, thin-border, limited-color-palette visual language across the app, not just the canvas from Phase 2.
4. Motion stays subtle: pane transitions and hover states, nothing bouncy or attention-seeking.
5. Test against the top-level bar for this phase: does this feel calm and easy to understand, or does it feel busy? If busy, simplify before moving on, don't add more to compensate.

## 8. Phase 6 — Auth and backend consolidation (only if asked)

Do not do this phase unless it's explicitly requested separately. By default, Firebase Auth stays as it is and only the data layer moves to Postgres/Supabase. If later asked to consolidate auth into Supabase too, treat that as its own scoped task with its own review, not something to fold into this migration.

## 9. Testing checklist before calling this done

- A note with a deep branch tree (at least 4 levels of forking) renders correctly and stays readable in the canvas.
- Forking, reviewing, and merging a branch works end to end through the actual UI, not just at the data layer.
- Offline fork-then-sync does not lose data (see Phase 4).
- The canvas never shows more than one note's checkpoints at a time.
- No leftover `d3-force` dependency or dead code from the old graph view, unless Phase 0 found it used elsewhere, in which case that separate usage is untouched and noted.

## 10. What's explicitly out of scope for this migration

- A cross-note graph or tag-based linking view (see `orrery-skills.md`, this is a permanent non-goal, not a "later" item).
- Theme customization or user-facing style settings.
- Any modality-specific logic per note type (story vs. decision log vs. research note). The checkpoint model stays the same underneath all of them.