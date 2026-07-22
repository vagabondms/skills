# Plan Review Format

The plan review is a single self-contained HTML file in the OS temp directory. **Everything is inline — no CDN, no external fonts, no scripts.** The file must render identically from `file://`, offline, and inside the Claude app's viewer, which blocks network fetches; a stylesheet that arrives over the network is a stylesheet that silently doesn't.

## Scaffold

```html
<!doctype html>
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <title>Plan review — {{plan title}}</title>
    <style>
      /* base layer — self-contained, tuned to the same editorial look every review shares */
      :root {
        --ink: #0f172a; --muted: #64748b; --line: #e2e8f0;
        --bg: #fafaf9; --card: #ffffff; --accent: #4f46e5;
        --amber-bg: #fef3c7; --amber-ink: #92400e;
        --emerald-bg: #d1fae5; --emerald-ink: #065f46;
        --red-bg: #fee2e2; --red-ink: #991b1b;
        --slate-bg: #f1f5f9; --slate-ink: #475569;
      }
      * { box-sizing: border-box; }
      body { margin: 0; background: var(--bg); color: var(--ink); line-height: 1.6;
             font-family: ui-sans-serif, system-ui, -apple-system, "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif; }
      main { max-width: 56rem; margin: 0 auto; padding: 3rem 1.5rem; display: flex; flex-direction: column; gap: 3rem; }
      h1 { font-size: 1.875rem; margin: 0 0 0.25rem; letter-spacing: -0.02em; }
      h2 { font-size: 1.25rem; margin: 0 0 1rem; }
      h3 { font-size: 1.05rem; margin: 0; }
      p { margin: 0.5rem 0; }
      a { color: var(--accent); text-decoration: none; }
      .meta, .goal { color: var(--muted); font-size: 0.9rem; }
      .mono, code { font-family: ui-monospace, SFMono-Regular, Menlo, monospace; font-size: 0.8125rem; }
      .badge { display: inline-block; padding: 0.15rem 0.6rem; border-radius: 9999px; font-size: 0.75rem; font-weight: 600; vertical-align: middle; }
      .badge.awaiting, .badge.change { background: var(--amber-bg); color: var(--amber-ink); }
      .badge.new { background: var(--emerald-bg); color: var(--emerald-ink); }
      .badge.impl { background: var(--slate-bg); color: var(--slate-ink); }
      .badge.deleted { background: var(--red-bg); color: var(--red-ink); }
      ol.build-order { margin: 0; padding-left: 1.25rem; display: flex; flex-direction: column; gap: 0.75rem; }
      article.card { background: var(--card); border: 1px solid var(--line); border-radius: 0.75rem; padding: 1.25rem 1.5rem; display: flex; flex-direction: column; gap: 0.75rem; }
      #change-surface { display: flex; flex-direction: column; gap: 2rem; }
      pre { background: var(--card); border: 1px solid var(--line); border-radius: 0.5rem; padding: 0.75rem 1rem; overflow-x: auto; }
      table { border-collapse: collapse; width: 100%; font-size: 0.9rem; background: var(--card); border: 1px solid var(--line); }
      th, td { text-align: left; padding: 0.5rem 0.75rem; border-bottom: 1px solid var(--line); }
      th { color: var(--muted); font-weight: 600; font-size: 0.8rem; }
      .callout { background: var(--amber-bg); color: var(--amber-ink); border-radius: 0.5rem; padding: 1rem 1.25rem; }
      .callout ul { margin: 0.25rem 0 0; padding-left: 1.1rem; }
      footer { color: var(--muted); font-size: 0.875rem; border-top: 1px solid var(--line); padding-top: 1rem; }
      /* diff layer — keep exactly this, so every review reads the same */
      .diff { font-family: ui-monospace, SFMono-Regular, Menlo, monospace; font-size: 0.8125rem; line-height: 1.7;
              background: var(--card); border: 1px solid var(--line); border-radius: 0.5rem; padding: 0.75rem 0; overflow-x: auto; }
      .diff div { padding: 0 0.75rem; white-space: pre; }
      .diff .add { background: #dcfce7; color: #14532d; }
      .diff .del { background: #fee2e2; color: #7f1d1d; }
      .diff .ctx { color: #64748b; }
      .diff .add::before { content: "+ "; color: #16a34a; }
      .diff .del::before { content: "− "; color: #dc2626; }
      .diff .ctx::before { content: "  "; }
    </style>
  </head>
  <body>
    <main>
      <header>...</header>
      <section id="summary">...</section>
      <section id="build-order">...</section>
      <section id="change-surface">...</section>
      <section id="data-model">...</section>
      <section id="errors">...</section>
      <section id="open-questions">...</section>
      <footer id="gate">...</footer>
    </main>
  </body>
</html>
```

## Header

Plan title, repo name, date (`class="meta"`), and an amber `<span class="badge awaiting">Awaiting approval</span>` badge. One line under the title (`class="goal"`): the goal in ≤15 words. No introduction paragraph — straight into the plan.

## Summary & decisions

Two or three sentences on what is being built and why. Then **Decisions** — the resolved branches of the grill, one line each. This is the record of *why* the diff below looks the way it does; someone reading only this section should still nod along.

## Build order

An ordered list (`class="build-order"`) — the numbered steps of the plan. Each step: what it does, the modules it touches (`class="mono"`, anchor-linked to their change-surface cards), and the check that proves it landed (a test, a typecheck, a command going green).

## Change surface (the centrepiece)

One card (`<article class="card">`) per module touched. The diffs carry the weight; prose is sparse.

- **Title** — the module, named in the project's domain language.
- **Badge** — `<span class="badge new">new module</span>`, `<span class="badge change">interface change</span>`, `<span class="badge impl">implementation only</span>`, or `<span class="badge deleted">deleted</span>`.
- **Files** — monospaced list, `class="mono"`.
- **The interface diff**:
  - `new module` — the full proposed interface, every line `add`.
  - `interface change` — current signature lines as `del`, proposed as `add`, unchanged neighbours as `ctx`. Pull the current lines from the actual code — the red side is real, never reconstructed from memory.
  - `implementation only` — no diff. One line: *"interface unchanged — the seam holds"*, plus a sentence on what moves inside.
  - `deleted` — the disappearing interface, every line `del`, and one line naming where each caller lands instead.
- **One sentence** — the module's responsibility.
- **One sentence** — what sits across its seam (callers, adapters, tests).

```html
<div class="diff">
  <div class="ctx">export interface SessionStore {</div>
  <div class="del">  get(id: string): Session | undefined;</div>
  <div class="add">  get(id: string): Promise&lt;Session | null&gt;;</div>
  <div class="ctx">}</div>
</div>
```

Escape `<` and `>` inside diff lines (`&lt;`, `&gt;`) — a raw generic parameter silently disappears into the DOM.

The diff shows the **interface** — signatures, types, endpoint shapes — not imagined implementation bodies. When a body sketch is genuinely load-bearing (a state machine, a reducer), keep it ≤10 lines in a plain code block labelled *sketch*, outside the diff.

## Data model

The core entity shapes as real type declarations in one code block. When the relationships are genuinely graph-shaped, draw a small **inline `<svg>`** — hand-rolled boxes and arrows are enough. No Mermaid, no CDN import: a diagram that needs the network is a diagram that renders blank in the app viewer and offline.

## Errors and edge cases

A compact table: seam · what can fail · how the failure surfaces in the types. Rows come from the session; a gap here is an open question, not a guess.

## Open questions

Amber callout box (`class="callout"`), one bullet per deferred decision. Deferred means listed here — never silently dropped.

## The gate (footer)

One quiet line: _"Review the diff, then approve in the terminal — implementation starts on your word. Signatures above are contracts."_

## Style

- Lean editorial, not corporate-dashboard: generous whitespace, one accent colour (indigo or emerald), amber for open questions, red only inside diffs.
- The review reads top-to-bottom in under two minutes. A card that needs a paragraph to be understood needs a better diff instead.
- Static HTML, no app code, **no scripts and no external resources of any kind** — inline CSS only, system font stacks, inline SVG for the rare diagram.
- When the plan touches code, prose speaks the `/codebase-design` vocabulary — module, interface, seam, adapter, depth, locality — and the project's own domain terms. No hedging, no throat-clearing.
