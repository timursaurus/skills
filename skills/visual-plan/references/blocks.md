# Block catalog

Copy-paste building blocks for a visual plan. Every snippet uses CSS classes that
already exist in [`template.html`](template.html) — so paste a block into the
template's `.container` and it is styled with no extra CSS.

**Assembly:** start from `template.html`, keep the blocks you need, fill them with
real content, and delete the rest (they're independent). Order them to read top to
bottom like a plan: header → summary → problem → approach → diagram → decisions →
file changes → phases → mockup → risks → open questions.

Each block below is wrapped in a `<!-- BLOCK: name -->` marker in the template so you
can find and replace it in place.

---

## Header / meta

Title, one-line summary, and status chips. Always include it — it frames the plan.
Chip modifiers: `chip--info` (status), `chip--ok` (good/cheap), `chip--warn` (caution). Plain `chip` for neutral facts.

```html
<header class="vp-header">
  <p class="vp-eyebrow">Visual Plan</p>
  <h1 class="vp-title">Plan title</h1>
  <p class="vp-summary">One sentence: what this does and the core move.</p>
  <ul class="chips">
    <li class="chip chip--info">Draft</li>
    <li class="chip">Scope: area</li>
    <li class="chip chip--ok">Effort: ~2 days</li>
    <li class="chip chip--warn">1 open question</li>
  </ul>
</header>
```

## Summary callout (TL;DR)

The one-paragraph version for someone who reads nothing else. Use right after the header.

```html
<div class="callout callout--summary">
  <p class="callout__label">Summary</p>
  <p>Two or three sentences capturing the whole approach and why it works.</p>
</div>
```

## Problem / context

Why this change exists. Keep it to the facts that justify the work.

```html
<section class="vp-section">
  <h2>Problem</h2>
  <p>What's broken or missing, and the evidence (metrics, tickets, constraints).</p>
</section>
```

## Approach

The strategy in prose — how the pieces fit, what's reused, what's new.

```html
<section class="vp-section">
  <h2>Approach</h2>
  <p>The plan in a few sentences. Reference real files with <code>inline code</code>.</p>
</section>
```

## Key decisions table

Make the hard-to-reverse choices explicit, with rationale and the alternatives you rejected.

```html
<section class="vp-section">
  <h2>Key decisions</h2>
  <table class="vp-table">
    <thead><tr><th>Decision</th><th>Choice</th><th>Why</th><th>Alternatives</th></tr></thead>
    <tbody>
      <tr><td>What you decided</td><td>The choice</td><td>The reason</td><td>What you didn't pick</td></tr>
    </tbody>
  </table>
</section>
```

## File-changes table

Concrete edits at a glance. Tag each row `tag--add`, `tag--edit`, or `tag--del`.

```html
<section class="vp-section">
  <h2>File changes</h2>
  <table class="vp-table">
    <thead><tr><th>Path</th><th>Change</th><th>Notes</th></tr></thead>
    <tbody>
      <tr><td><code>path/to/new.ts</code></td><td><span class="tag tag--add">add</span></td><td>What it does</td></tr>
      <tr><td><code>path/to/edit.ts</code></td><td><span class="tag tag--edit">edit</span></td><td>What changes</td></tr>
      <tr><td><code>path/to/old.ts</code></td><td><span class="tag tag--del">delete</span></td><td>Why it goes</td></tr>
    </tbody>
  </table>
</section>
```

## Phase / step timeline

Ordered, dependency-respecting implementation steps. Number phases sequentially.

```html
<section class="vp-section">
  <h2>Implementation phases</h2>
  <ol class="timeline">
    <li class="phase">
      <span class="phase__num">1</span>
      <div class="phase__body">
        <p class="phase__title">Phase name</p>
        <ul><li>Task</li><li>Task</li></ul>
      </div>
    </li>
    <li class="phase">
      <span class="phase__num">2</span>
      <div class="phase__body">
        <p class="phase__title">Next phase</p>
        <ul><li>Task</li></ul>
      </div>
    </li>
  </ol>
</section>
```

## Architecture diagram

Two ways to draw flow/architecture. **Default to the offline version** — it always
renders with zero network. Reach for Mermaid only when a richer flowchart, sequence,
or ER diagram earns its keep.

### Offline (boxes-and-arrows — always renders)

Highlight the key node with `node--accent`. Use `&rarr;` / `&darr;` for arrows.

```html
<section class="vp-section">
  <h2>Flow</h2>
  <div class="diagram">
    <div class="flow">
      <div class="node">Step one</div>
      <span class="arrow">&rarr;</span>
      <div class="node">Step two</div>
      <span class="arrow">&rarr;</span>
      <div class="node node--accent">Result</div>
    </div>
  </div>
</section>
```

### Mermaid (richer — needs internet to render)

First enable Mermaid: in `template.html`, uncomment the two `<script>` lines at the
bottom of `<body>`. Then use a `pre.mermaid` block. The source stays readable even if
the file is opened offline, so it degrades gracefully.

```html
<section class="vp-section">
  <h2>Flow</h2>
  <pre class="mermaid">
flowchart LR
  A[Email form] --> B[Sign token]
  B --> C[Send email]
  C --> D[Verify &amp; session]
  </pre>
</section>
```

## UI mockup frame

A browser-window frame around a wireframe. Use for any UI/frontend plan — seeing the
layout beats describing it. Build the inside from the wireframe primitives below.

```html
<section class="vp-section">
  <h2>UI mockup</h2>
  <div class="mockup">
    <div class="mockup__bar">
      <div class="mockup__dots"><span class="mockup__dot"></span><span class="mockup__dot"></span><span class="mockup__dot"></span></div>
      <span class="mockup__title">app.example.com/path</span>
    </div>
    <div class="mockup__body">
      <!-- wireframe primitives go here -->
    </div>
  </div>
</section>
```

### Wireframe primitives

Mix these inside `mockup__body`. `wf-text` is a skeleton line; width helpers `w-40 / w-60 / w-80`.

```html
<!-- top nav -->
<div class="wf-nav"><strong>Brand</strong><span>Link</span><span>Link</span></div>

<!-- a row of fields / buttons -->
<div class="wf-row">
  <div class="wf-input">placeholder text</div>
  <span class="wf-btn">Primary action</span>
  <span class="wf-btn wf-btn--ghost">Secondary</span>
</div>

<!-- cards side by side -->
<div class="wf-row">
  <div class="wf-card"><div class="wf-text w-60"></div><div class="wf-text w-80"></div></div>
  <div class="wf-card"><div class="wf-text w-60"></div><div class="wf-text w-40"></div></div>
</div>

<!-- skeleton text lines -->
<div class="wf-text w-80"></div>
<div class="wf-text w-60"></div>
```

## Code snippet

Show a key signature, schema, or config. Keep it short — a snippet, not a file dump.

```html
<pre class="code">function signToken(email: string): string {
  // HMAC over { email, exp }, base64url
}</pre>
```

## Risks & trade-offs

The honest downsides and what mitigates each. Use the `callout--risk` style.

```html
<section class="vp-section">
  <h2>Risks &amp; trade-offs</h2>
  <div class="callout callout--risk">
    <p class="callout__label">Risks</p>
    <ul>
      <li><strong>Risk name</strong> — the consequence, then the mitigation.</li>
    </ul>
  </div>
</section>
```

## Open questions / decisions for the user

Anything the plan can't settle on its own — surface it, don't bury it. Use `callout--question`.

```html
<section class="vp-section">
  <h2>Open questions</h2>
  <div class="callout callout--question">
    <p class="callout__label">For you to decide</p>
    <ul>
      <li>The question, framed as a clear either/or where possible.</li>
    </ul>
  </div>
</section>
```
