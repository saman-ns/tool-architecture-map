---
name: architecture-map
description: Map a code repo's architecture as a single self-contained HTML C4-style container diagram — labeled tier boxes (frontend/backend/data/external/MCP) with components nested inside and labeled arrows showing how they talk. Use when the user wants to "map the architecture", "visualize the codebase", "show me how this app is wired", "draw the stack", or "make an architecture diagram" of a repo.
---

# Architecture Map

Map a code repo as one self-contained, interactive HTML **C4 container diagram**. Every box, pill, and arrow must trace to code that actually runs — never invent one. Use your own judgement for anything not spelled out here.

## Investigate
**Study the code, never the prose.** Do not trust README/markdown/docs — or even code comments — to decide what exists; they drift and describe things that were removed or never shipped. Read the actual source: imports, call sites, config, and the dependency manifests. A box or edge is real only if running code backs it (a file, an import, a wired call, a declared dependency), not because a doc or comment claims it.

Read the repo enough to understand how it's actually wired — cheaply, not file-by-file. Look at: the file tree, manifests, frameworks, config, entry points, routes/wiring. For a large repo, fan out `Explore` subagents (one per area) and keep the conclusions, not the file dumps.

Sort what you find into tiers — **frontend · backend · data · external · infra · mcp**, plus any other tier the repo genuinely has. Name only the tiers that exist.

## Model, then gate
Build the structure first: tiers as boxes, modules grouped by responsibility as pills inside them, arrows for real relationships (label each with its protocol). Stay at the container level — a readable handful of boxes and edges, not a call graph. Show the user a short outline + the detected stack and **wait for approval** before building. (Headless caller: print it, then proceed.)

## Build
Copy `assets/template.html` and fill **only** the `MODEL` object — the engine draws everything (orthogonal non-overlapping arrows, drill-down, network toggle, pan/zoom, draggable cards). Don't touch the engine or hand-write boxes. Match the **"Stage" house style** (Inter font, `--accent` theming, neutral palette, no rainbow slop) — `references/c4-style.md` has the full `MODEL` schema + visual system. Save as `<repo>/architecture-map.html`.

What goes in each card (per node in `MODEL`):
- **box** — label, tech, layer, kind (`container` has pills · `node` is a bare box).
- **pills** — the modules/responsibilities inside a container.
- **drill-down detail** (read the real code for these) — a 1–2 line summary, the real file paths, a short flat list of sub-items if any (cap ~8), what it talks to, the entry file, and any networking facts (ports/domains/TLS).

## Outcome
One self-contained HTML file: labeled tier boxes with nested components, protocol-labeled arrows, click-to-drill-in detail, a networking toggle, and pan/zoom/drag — something a person can open and actually understand how the app is wired.

## Double-check (correctness above all)
Render it (Playwright MCP, headless, screenshot) and confirm it is a **correct and complete** representation:
- Account for **every** manifest dependency and **every** top-level dir — each is on the map, folded into a component, or deliberately out of scope. Anything unaccounted for is a miss; fix it.
- Nothing is fabricated: a doc or comment claim is **not** evidence. If no running code uses something, it's out of scope even if a stale dir or file lingers (e.g. an unreferenced data dir for a dependency that isn't installed).
- Arrows connect the right boxes, labels are legible, and drill-down + the network toggle work.

Report the stack, what you drew, the saved path, and anything you couldn't resolve.
