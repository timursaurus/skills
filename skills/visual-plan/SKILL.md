---
name: visual-plan
description: Render an implementation or architecture plan as a single self-contained HTML page the user opens in a browser — diagrams, file-change tables, phase timelines, and UI mockups instead of a wall of text. Use when a plan benefits from being seen rather than read — frontend / UI work, system architecture worth diagramming, multi-phase efforts, or any plan that will be shared, reviewed, or revisited. Trigger on "visual plan", "/visual-plan", "make the plan visual", "html plan", "render the plan as a page", "visualize the plan", "show me the plan as a webpage". Gate hard — it's the most expensive plan form, so skip it for trivial or throwaway changes where a plain text plan is cheaper and just as clear.
---

# Visual Plan

Turn a plan into one self-contained HTML page that opens in any browser — no build step, no server, no dependencies. The page carries the same backbone as a strong text plan (problem, approach, decisions, file changes, phases, risks, open questions) but adds what prose can't: architecture diagrams, file-change tables, a phase timeline, and clickable-looking UI mockups. The result is skimmable, shareable with people who don't read code, and easy to revisit.

This is a planning method, not a platform — any agent that can read files and write one file can produce the artifact. Prefer the harness's native tools where they exist (file preview, browser-open, plan approval), and degrade gracefully to a file path where they don't.

## When to use

Gate hard. A polished visual plan is the most expensive plan form, so only invest when seeing the plan beats reading it:

- **Frontend / UI work** — wireframe the screen instead of describing it.
- **System architecture** — a diagram of components and flow carries more than paragraphs.
- **Multi-phase efforts** — a timeline makes sequence and dependencies obvious.
- **Plans that travel** — anything shared with non-coders, presented for sign-off, or revisited weeks later.

Skip it for trivial or obvious changes (a rename, a one-file fix) and for throwaway plans — a plain text plan is cheaper and just as clear. Substance comes first: the reasoning is the plan, the visuals only present it. Never let polish stand in for a weak plan.

## The artifact

One self-contained `.html` file with an inline `<style>` block and no required JavaScript. It opens directly via `file://`, renders fully offline, and supports light and dark mode. Write it to a predictable path — `visual-plan.html` at the repo root, or `.plans/<slug>.html` for a scratch location.

## Producing the plan

1. **Research first.** Explore the codebase and gather context with the same rigor as any good plan. Decide here whether a visual plan is even warranted (see *When to use*); if not, say so and write a text plan instead.
2. **Draft the content.** Settle the substance before touching HTML: problem, approach, key decisions with rationale, file changes, phased steps, risks and trade-offs, and any open questions only the user can resolve.
3. **Render.** Start from [`references/template.html`](references/template.html) and assemble the page by copying the blocks you need from [`references/blocks.md`](references/blocks.md). Fill each with real content and delete the blocks that don't apply — they're independent.
4. **Mockups and diagrams.** For UI work, build HTML/CSS wireframes inside a mockup frame. For architecture or flow, use the offline boxes-and-arrows pattern, or a Mermaid block when a richer diagram earns it.
5. **Write and surface.** Write the file, then give the user the path and how to open it — a native preview/open tool if the harness has one, otherwise `open` (macOS) / `xdg-open` (Linux) / `start` (Windows), otherwise just the path. Never only point at a file; tell the user what's in it.
6. **Revise.** On feedback, edit the HTML in place and re-surface. The file is the living artifact.

## Building blocks

The catalog in [`references/blocks.md`](references/blocks.md) holds a copy-paste snippet for each block, each using CSS classes already defined in [`references/template.html`](references/template.html) — so a pasted block is styled with no extra CSS. Available blocks: header/meta, summary callout, problem, approach, key-decisions table, file-changes table, phase timeline, architecture diagram, UI mockup frame with wireframe primitives, code snippet, risks & trade-offs, and open questions. Keep the class names in the template and the catalog in sync.

## Diagrams

Default to the offline boxes-and-arrows pattern — pure HTML/CSS that always renders with zero network. Use Mermaid (loaded from a CDN) only when a richer flowchart, sequence, or ER diagram is worth the dependency; enabling it is uncommenting two lines in the template. When you use Mermaid, keep the diagram source readable so the file still communicates if it's opened offline.

## Reusable across agents

The skill needs only three capabilities: read files, write one file, and (optionally) open a browser. There is no MCP server, no account, and no platform lock-in. Where the harness offers native tools — file preview, browser-open, a plan/approval step — prefer them; where it doesn't, fall back to printing the path. The visual plan also composes with other planners: render the synthesis from `dual-plan`, or any text plan, into this format as a final presentation step.

## Notes

- Keep it to **one** self-contained file with inline CSS — portability is the point.
- Don't pin a model version; run on the strongest reasoning model available, since this exists to present decisions that are expensive to get wrong.
- Graceful degradation: if a diagram tool or browser-open isn't available, ship the offline version and the path rather than aborting.
- Substance over polish — a beautiful page can't rescue a plan that hasn't done the thinking.
