
Give this to the agent along with `orrery-skills.md`. This replaces `orrery-build-prompt.md` as the main task document, it covers the same ground but at the level of detail actually needed to build against: every page, the sidebar on each one, the full editor feature list, the graph UI in detail, the backend, which existing components to use instead of building from zero, and where terminal commands and API keys come into play.

A note on the specific package names and library recommendations below: I'm writing this without being able to verify current versions against the web right now, a search error on my end. Treat package names, exact version numbers, and "is this still maintained" claims as needing a quick check against current docs before you run anything, not as confirmed fresh. I've flagged the ones most likely to have moved.

---

## 1. Decisions made here, not left open

Two decisions from earlier are still genuinely open, don't resolve them silently:

- **How a checkpoint gets created** (manual marking versus automatic extraction from the writing).
- **Sync transport** (self-hosted `y-websocket` versus a hosted service like Liveblocks or y-sweet). This one has a concrete technical consequence worth knowing before you decide: Vercel's serverless functions don't hold long-lived WebSocket connections open. If you self-host, the sync server has to run somewhere that supports a persistent process, Fly.io, Render, a small VPS, not a Vercel API route. A hosted sync service exists specifically to take that infrastructure problem off your hands. That's the real shape of the tradeoff, not just "more control versus less setup."

One more, decided here rather than left open: **canvas nodes are not manually draggable.** Position comes entirely from the automatic tree layout. Letting people manually rearrange checkpoint nodes adds a state to persist, a way for the diagram to get messy over time, and works against "calm, not overwhelming." If this turns out to feel too rigid once it's built, it's easy to add drag support later, harder to claw back a layout that's already gone messy for existing users.

## 2. Pages and routes

- **`/sign-in`, `/sign-up`** — Firebase Auth, email/password and Google OAuth. Can be one page with a toggle instead of two routes if that's simpler.
- **`/` (home, authenticated)** — note list, no note open. Empty state if there are no notes yet.
- **`/notes/[id]`** — the main app view. Editor by default, with a toggle to switch that note's view to the checkpoint canvas (not a separate route, a view-mode toggle within the same page, since the canvas is a way of looking at one note, not a destination of its own).
- **`/settings`** — account info, sign out, AI model preference (which OpenRouter model to use for generation), sync/device status.

That's the full route list. No separate route for the canvas, no route for a cross-note graph (there isn't one, see `orrery-skills.md`).

## 3. Global layout structure

Every authenticated page shares the same shell: a left sidebar and a main content area. The sidebar is persistent, it doesn't change structurally between pages, only its content does.

Sidebar zones, top to bottom:

1. Search (opens the command palette, see below, rather than an inline search box)
2. "New note" action
3. The note list or folder tree, scrollable, current note highlighted when one is open
4. Account/settings entry pinned at the bottom

On the canvas view specifically, collapse the sidebar to its icon-only width by default, the canvas needs the room and the person already navigated there from a note, they don't need the full list open at the same time. Expandable back with one click.

On mobile widths, the sidebar becomes a drawer that opens over the content rather than living beside it permanently.

## 4. Sidebar functionality, page by page

- **Home**: full note list, search, new note. This is the default, richest state of the sidebar.
- **Note editor**: same note list, current note highlighted. A separate, contextual right-hand panel (not the sidebar) shows that note's checkpoint list as a flat list, useful for jumping around without opening the full canvas.
- **Canvas view**: sidebar collapsed to icons as above. The right-hand panel from the editor view goes away, the canvas itself is showing that same information spatially.
- **Settings**: sidebar can either stay as-is or be replaced by settings-specific sub-navigation (Account, AI preferences, Sync). Either is fine, pick whichever is less code given how the rest of the shell is built.

## 5. Editor and formatting (the Obsidian-like part)

Built on TipTap. Full formatting feature list to support:

- Headings (H1 through H6)
- Bold, italic, strikethrough, inline code
- Blockquotes
- Bullet lists, numbered lists, nested lists
- Task lists / checkboxes
- Tables
- Horizontal rule
- Links
- Pasted or dragged images
- Code blocks with syntax highlighting (TipTap's lowlight-based code block extension covers this)
- Wiki-style `[[links]]` between notes, this needs a custom extension built on TipTap's suggestion utility (`@tiptap/suggestion`), it's not a stock extension, budget real time for it
- `#tags`, same situation, a custom extension following the same suggestion pattern as wiki-links

Nice-to-have, lower priority, include if time allows rather than in the first pass:

- Callout blocks (Obsidian's `>[!note]`, `>[!warning]` style boxes)
- Footnotes
- Inline and block math via KaTeX
- Mermaid diagram code blocks

**Block insertion**: a slash command menu (typing `/` opens a menu of block types to insert) is the primary way blocks get added, this is what makes the editor feel like Craft rather than a plain markdown textbox. `@tiptap/suggestion` again is the underlying mechanism, the same one wiki-links and tags use, so this isn't three separate systems, it's one pattern applied three times.

**Checkpoint marking**: whatever mechanism gets decided in section 1, it should be reachable from the slash command menu (`/checkpoint`) if manual, so it's consistent with how every other block gets inserted rather than being a separate UI paradigm.

Check TipTap's current documentation for the exact extension package names before installing, the extension surface has changed across versions and I can't confirm current names right now.

## 6. Graph/canvas UI, in detail

Built on `@xyflow/react` (React Flow) with `dagre` for automatic layout (check whether `dagre` or `elkjs` is currently the better-maintained option before committing, this is one of the things I couldn't verify today).

**Layout direction**: horizontal, left to right. Mainline checkpoints run in a straight horizontal line, branches fork downward (or upward) off the checkpoint they came from. This reads as "time moving forward," the same convention as most git-history visualizations. Vertical top-to-bottom is a reasonable alternative if it fits your specific screen proportions better, this is a recommendation, not a hard rule.

**Node anatomy**: a small flat card per checkpoint, rounded corners, thin 1px border, no drop shadow. Shows a short text preview of that checkpoint's content, not the full text. Mainline nodes and branch nodes are visually distinguishable through a subtle difference (border color or a small icon), not through size or heavy color contrast, the tree structure itself should communicate mainline-versus-branch, the color is a secondary confirmation, not the primary signal.

**Edges**: simple lines between nodes, a slight curve rather than sharp right angles.

**Interactions**:

- Pan and zoom, standard React Flow behavior.
- Click a node to jump the editor to that point in the document (scroll sync between canvas and editor).
- Hovering a node reveals a "fork from here" action, this is the main way forking gets triggered from the canvas itself (also reachable from the editor directly, see section 5).
- No manual node dragging, per the decision in section 1.

**Controls and minimap**: React Flow ships built-in `Controls` and `MiniMap` components, use them, but restyle them to match the flat, thin-border visual language rather than shipping their default appearance, which doesn't match "calm."

**Empty state**: a note with no forks yet (just the mainline) doesn't need the canvas prominently surfaced, a small subtle indicator (an icon, maybe a count) that a checkpoint tree exists is enough. The canvas becomes actually useful once branches exist, don't force it into view before then.

## 7. Backend

**Auth**: Firebase Auth. Client gets a session token, sent with API requests, verified server-side (Firebase Admin SDK) before touching any data.

**API layer**: Next.js API routes for everything except the sync transport (see section 1's note on why that can't live on Vercel serverless if self-hosted). Routes needed, roughly:

- Note CRUD
- Checkpoint CRUD
- Fork/generate (calls OpenRouter server-side, see the security note below)
- Merge

**Database**: Postgres via Supabase. Core tables: notes, checkpoints (with `parent_checkpoint_id` self-referencing the same table, per the data model in `orrery-skills.md`). Use Supabase row-level security so every row is scoped to the owning user, don't rely on the API layer alone to enforce that.

**Security note that matters**: the OpenRouter API key must never reach the client. All generation calls go through your own API route, which holds the key server-side and proxies the request. If you find OpenRouter's key anywhere in client-side code or a `NEXT_PUBLIC_` environment variable, that's a mistake to fix immediately, not a shortcut to leave in.

## 8. Don't build from scratch, what to lean on instead

- **UI primitives and component patterns**: Radix UI (already in the stack) plus shadcn/ui's component patterns built on top of it, sidebar, dialog, dropdown, command palette shells are all things shadcn has polished, human-designed versions of. Copy the pattern, restyle to match `orrery-skills.md`'s flat, thin-border language, don't design these from zero.
- **Command palette (⌘K)**: `cmdk` is the library shadcn's own command menu is built on, worth using directly rather than building keyboard-driven fuzzy search and focus management by hand.
- **Icons**: Phospher (already chosen), consistent flat line-icon style, matches the visual language you're going for.
- **Editor foundation**: TipTap itself, plus its official extensions for anything stock (tables, task lists, code blocks). Only build custom extensions for the genuinely custom pieces, wiki-links, tags, checkpoint markers.
- **Graph layout**: `dagre` or `elkjs`, don't hand-roll tree layout math.
- **Reference product for the combination you're going for**: AFFiNE is an open-source app that combines a block-based document editor with a canvas view in one product, worth looking at directly (its UI, and possibly parts of its open-source code) as a reference for how an editor-plus-canvas app can feel coherent rather than like two separate tools bolted together. Confirm its current license terms before reusing any actual code, not just the ideas.

## 9. Terminal commands, in order

**Phase 0, project setup**:

```
npx create-next-app@latest orrery --typescript --tailwind --app
cd orrery
npm install @tiptap/react @tiptap/pm @tiptap/starter-kit zustand dexie framer-motion lucide-react @xyflow/react dagre cmdk firebase
```

Check TipTap's docs for the current names of the table, task-list, and code-block extension packages before adding them, and confirm `dagre` is still the recommended layout package for React Flow at the time you're doing this, versus `elkjs`.

**Supabase setup**:

```
npx supabase init
npx supabase start
```

(local development database). For the actual schema:

```
npx supabase migration new create_notes_and_checkpoints
```

then write the migration SQL by hand, and:

```
npx supabase db push
```

to apply it.

**Firebase setup**: mostly done in the Firebase console (create a project, enable Auth providers), not the terminal. `npm install firebase` on the client side, and `npm install firebase-admin` for the server-side token verification in your API routes.

**Deployment, once everything works locally**:

```
npx vercel
```

for a preview deploy, `npx vercel --prod` when ready for production. Supabase's production database is configured through their dashboard, not additional terminal commands, beyond pushing migrations to it the same way as local.

## 10. API keys and secrets, what and when

| Key                                                       | When you need it                                             | Client-safe or server-only                                                             |
| --------------------------------------------------------- | ------------------------------------------------------------ | -------------------------------------------------------------------------------------- |
| Firebase web config (apiKey, authDomain, projectId, etc.) | Phase 0/1, auth setup                                        | Client-safe, this is normal Firebase web config, not a secret in the traditional sense |
| Firebase Admin SDK service account key                    | Phase 1, server-side token verification                      | Server-only, never expose                                                              |
| Supabase project URL + anon key                           | Phase 0/2                                                    | Client-safe                                                                            |
| Supabase service role key                                 | Phase 2 onward, privileged server operations                 | Server-only, never expose                                                              |
| OpenRouter API key                                        | Phase 6, fork/generate                                       | Server-only, never expose, see the security note in section 7                          |
| Liveblocks or y-sweet API key                             | Phase 8, only if you choose a hosted sync service            | Depends on the service, check their docs, don't assume client-safe by default          |
| Vercel token                                              | Phase 10, only if deploying via CLI instead of the dashboard | Server-only (used locally in your terminal, not embedded in the app)                   |

## 11. Testing checklist

- A note with a deep branch tree (4+ levels) renders and stays readable in the canvas.
- Fork, review, and merge works end to end through the real UI, not just at the data layer.
- Offline fork-then-sync doesn't lose data.
- The canvas never shows more than one note's checkpoints at once.
- No API key appears anywhere in client-side bundle output, check the built output, not just the source.
- Auth gates every route and every API endpoint, not just the UI.

## 12. Explicitly out of scope

- A cross-note graph or tag-based linking view.
- Theme customization or user-facing style settings.
- Manual node dragging on the canvas (see section 1).
- Any modality-specific logic per note type.