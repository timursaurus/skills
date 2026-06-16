## Skills

### [dual-plan](skills/dual-plan/SKILL.md)

Adversarial planning. Two planners draft solutions from genuinely opposing worldviews — a pragmatic architect (Alpha) and a systems thinker (Beta) — then each cross-reviews the other's plan, and a synthesizer merges the strongest ideas into one debate-tested plan. The value comes from real disagreement: a single pass commits early and rationalizes, while two live options stay on the table until the evidence picks a winner.

**Use it** for system architecture, non-obvious feature work, refactoring strategies, API / data / state design, and high-stakes choices that are expensive to undo. Skip it for trivial changes with one sensible approach.

The method runs in three rounds across five roles:

1. **Independent plans** — two planners explore the codebase and produce structured plans from their assigned personas.
2. **Cross-review** — each reviewer stress-tests the *other* planner's plan, grounding every point in real files and patterns.
3. **Synthesis** — one synthesizer combines the best of both, resolves the critiques, and flags any disagreement the evidence can't settle as a live choice for the user.

Trigger it with phrases like "dual plan", "debate plan", "adversarial plan", "argue the approach", or `/dual-plan` — or just ask for a thorough, stress-tested plan on a decision with real trade-offs.

## Installing a skill

```bash
npx skills add timursaurus/skills
```

```bash
npx skills add https://github.com/timursaurus/skills --skill dual-plan
```
