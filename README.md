## Skills

### [dual-plan](skills/dual-plan/SKILL.md)

Adversarial planning. Two planners draft solutions from genuinely opposing worldviews — a pragmatic architect (Alpha) and a systems thinker (Beta) — then each cross-reviews the other's plan, and a synthesizer merges the strongest ideas into one debate-tested plan. The value comes from real disagreement: a single pass commits early and rationalizes, while two live options stay on the table until the evidence picks a winner.

**Use it** for system architecture, non-obvious feature work, refactoring strategies, API / data / state design, and high-stakes choices that are expensive to undo. Skip it for trivial changes with one sensible approach.

The method runs in three rounds across five roles:

1. **Independent plans** — two planners explore the codebase and produce structured plans from their assigned personas.
2. **Cross-review** — each reviewer stress-tests the _other_ planner's plan, grounding every point in real files and patterns.
3. **Synthesis** — one synthesizer combines the best of both, resolves the critiques, and flags any disagreement the evidence can't settle as a live choice for the user.

Trigger it with phrases like "dual plan", "debate plan", "adversarial plan", "argue the approach", or `/dual-plan` — or just ask for a thorough, stress-tested plan on a decision with real trade-offs.

### [visual-plan](skills/visual-plan/SKILL.md)

Render a plan as a single self-contained HTML page you open in a browser — diagrams, file-change tables, a phase timeline, and UI mockups instead of a wall of text. No build step, no server, no dependencies: the agent writes one `.html` file and you open it. A simpler, platform-free take on the idea behind [BuilderIO's visual-plan](https://github.com/BuilderIO/skills/tree/main/skills/visual-plan).

**Use it** for frontend / UI work (wireframe the screen), system architecture worth diagramming, multi-phase efforts, and any plan that will be shared, reviewed, or revisited. Skip it for trivial changes — a plain text plan is cheaper.

How it works:

1. **Draft** the plan's substance — problem, approach, decisions, file changes, phases, risks, open questions.
2. **Render** it into a bundled HTML scaffold by copying from a catalog of pre-styled blocks.
3. **Open** the file in a browser, and revise it in place on feedback.

Diagrams render offline by default (pure HTML / CSS), with optional Mermaid charts when a richer flowchart helps. Reusable by any agent that can read and write files — and it composes with `dual-plan`: render a debate-tested synthesis as a visual page.

Trigger it with phrases like "visual plan", "make the plan visual", "html plan", or `/visual-plan`.

## Installing a skill

```bash
npx skills add timursaurus/skills
```
