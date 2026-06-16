---
name: dual-plan
description: Adversarial planning — two agents draft plans from opposing worldviews, cross-review each other, then a synthesizer merges the strongest ideas into one debate-tested plan. Use whenever the user wants to plan a feature, architect a system, design an implementation, or choose between approaches and wants a thorough, stress-tested result rather than a single off-the-cuff pass. Trigger on "dual plan", "debate plan", "adversarial plan", "two perspectives", "argue the approach", "/dual-plan", and — even without those words — on any complex architectural or design decision with real trade-offs and more than one defensible answer. When in doubt between a quick plan and this, prefer this; the cost is a few extra minutes and the payoff is catching the flaw a single pass would have shipped.
---

# Dual Plan

Adversarial planning. Two planners propose solutions from genuinely different worldviews, each critiques the other's plan, and a synthesizer combines the best of both. The value comes from real disagreement: a single pass commits early and rationalizes, while two live options stay on the table until the evidence picks a winner.

## When to use

Use it where there are real trade-offs and more than one defensible answer: system architecture with competing concerns, feature work where the approach isn't obvious, refactoring strategies, API / data / state design, high-stakes choices that are expensive to undo. For trivial or obvious tasks (rename a variable, a one-file change with one sensible approach), skip it and just do the thing.

## Running the roles — prefer native tools

The method is five roles: two planners, two reviewers, one synthesizer. Use whatever your harness gives you, reaching for native tools first:

- **Native subagents + tools (ideal — e.g. Claude Code).** Spawn each role with the subagent tool, running the two planners in parallel and the two reviewers in parallel. The read-only **Plan** agent type fits planning and review well. Present the synthesizer's final plan through the native plan tool (**ExitPlanMode**) so the user can approve it and move straight to implementation. When the debate surfaces a decision only the user can settle, ask with the native question tool (**AskUserQuestion**) instead of guessing.
- **Subagents but no parallelism.** Same five roles, one at a time. Independent contexts still preserve the disagreement; it's just slower.
- **No subagents (you, solo).** Play each role in turn. This still catches errors a single pass misses, but guard against echoing yourself: write each artifact to a file *before* starting the next role, and approach each role as an adversary hunting for a materially different and better answer — not the last role with tweaks. If you genuinely can't produce a different Beta, the problem may have one obvious answer — say so.

Independent context per role is what makes the disagreement real. Keep the roles from anchoring on each other's framing wherever your harness allows.

## The three rounds

### Round 1 — Independent plans

Run two planners independently. Their personas live in separate files — read the matching one into each planner's prompt and have it commit fully to that worldview rather than hedging:

- **Alpha** → `references/persona-alpha.md` (pragmatic architect)
- **Beta** → `references/persona-beta.md` (systems thinker)

Each planner explores the codebase first, then produces a structured plan: problem statement, approach, key decisions with rationale, file changes, risks and trade-offs, and a phased implementation order. Persist each plan so the next round can read it.

### Round 2 — Cross-review

Each reviewer critiques the *other* planner's plan — Alpha reviews Beta, Beta reviews Alpha. The job is honest stress-testing, not contrarianism: name the genuine weaknesses and missed considerations, acknowledge the real strengths, and say specifically where your own approach would do better and why. Ground every point in actual files and patterns, not generalities. Persist each review.

### Round 3 — Synthesis

One synthesizer reads both plans and both reviews and produces the final plan — not by picking a winner, but by combining the strongest ideas and resolving the critiques. It should lock in where the plans agreed (high-confidence decisions), resolve each disagreement using the reviews as evidence, and either incorporate each critique's fix or explain why it doesn't apply. Where the reviews don't decisively settle a disagreement and either path is genuinely defensible, **don't force an arbitrary pick** — flag it as a live choice for the user. The final plan carries the same structure as Round 1, plus a short note on the key insight the debate produced and any decisions left open for the user.

## Presenting the result

Present the final plan to the user directly — never just point at a file path. In Claude Code, hand it off through **ExitPlanMode** so the user can approve and proceed to implementation in one step. Include a short summary of where the planners agreed, where they disagreed, and how each disagreement was resolved, and offer to show the raw plans and reviews. Whenever the synthesis couldn't decisively settle a disagreement — or an open question remains that only the user can answer — **prompt the user to pick** with **AskUserQuestion** rather than choosing for them. Offer the competing approaches as the choices, each with a one-line trade-off, so the user decides informed.

## Notes

- Use a disposable scratch workspace for artifacts — a writable directory like `.dual-plan/<run-id>/`, or carry artifacts as in-context text if you have no filesystem. The final plan is the deliverable; the workspace is throwaway.
- Give every planner and reviewer access to the codebase — they need to ground reasoning in the real architecture, not plan in the abstract. Thread any user-provided files into every role's prompt.
- Run every role on the strongest reasoning model available; this exercise exists to catch expensive mistakes, so don't economize here. Don't pin a specific model version — names go stale.
- Graceful degradation: if one role fails, note the gap and continue. The synthesizer can still produce a strong plan from one full plan plus a partial review — don't abort the whole run over one agent.
- **Ask, don't assume.** Keep guessing to a minimum. Where the answer turns on a fact or preference only the user has — and the codebase doesn't settle it — ask with AskUserQuestion instead of inventing it. An unfounded assumption shared by both planners survives into the synthesis unchallenged, so flag it as a question rather than baking it into both plans.
