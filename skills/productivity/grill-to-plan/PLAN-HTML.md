# Plan Review Format

The plan review is a single self-contained HTML file in the OS temp directory. Tailwind comes from its CDN; a small custom CSS layer renders the diff lines. Add the Mermaid CDN import (as in the `/improve-codebase-architecture` report) only when a diagram earns its place — most reviews need none.

## Scaffold

```html
<!doctype html>
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <title>Plan review — {{plan title}}</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
      /* diff layer — keep exactly this, so every review reads the same */
      .diff { font-family: ui-monospace, SFMono-Regular, Menlo, monospace; font-size: 0.8125rem; line-height: 1.7; }
      .diff div { padding: 0 0.75rem; white-space: pre; }
      .diff .add { background: #dcfce7; color: #14532d; }
      .diff .del { background: #fee2e2; color: #7f1d1d; }
      .diff .ctx { color: #64748b; }
      .diff .add::before { content: "+ "; color: #16a34a; }
      .diff .del::before { content: "− "; color: #dc2626; }
      .diff .ctx::before { content: "  "; }
    </style>
  </head>
  <body class="bg-stone-50 text-slate-900 font-sans">
    <main class="max-w-4xl mx-auto px-6 py-12 space-y-12">
      <header>...</header>
      <section id="summary">...</section>
      <section id="build-order">...</section>
      <section id="change-surface" class="space-y-8">...</section>
      <section id="data-model">...</section>
      <section id="errors">...</section>
      <section id="open-questions">...</section>
      <footer id="gate">...</footer>
    </main>
  </body>
</html>
```

## Header

Plan title, repo name, date, and an amber `Awaiting approval` badge. One line under the title: the goal in ≤15 words. No introduction paragraph — straight into the plan.

## Summary & decisions

Two or three sentences on what is being built and why. Then **Decisions** — the resolved branches of the grill, one line each. This is the record of *why* the diff below looks the way it does; someone reading only this section should still nod along.

## Build order

An ordered list — the numbered steps of the plan. Each step: what it does, the modules it touches (`font-mono text-sm`, anchor-linked to their change-surface cards), and the check that proves it landed (a test, a typecheck, a command going green).

## Change surface (the centrepiece)

One card (`<article>`) per module touched. The diffs carry the weight; prose is sparse.

- **Title** — the module, named in the project's domain language.
- **Badge** — `new module` (emerald), `interface change` (amber), `implementation only` (slate), or `deleted` (red).
- **Files** — monospaced list, `font-mono text-sm`.
- **The interface diff**:
  - `new module` — the full proposed interface, every line `add`.
  - `interface change` — current signature lines as `del`, proposed as `add`, unchanged neighbours as `ctx`. Pull the current lines from the actual code — the red side is real, never reconstructed from memory.
  - `implementation only` — no diff. One line: *"interface unchanged — the seam holds"*, plus a sentence on what moves inside.
  - `deleted` — the disappearing interface, every line `del`, and one line naming where each caller lands instead.
- **One sentence** — the module's responsibility.
- **One sentence** — what sits across its seam (callers, adapters, tests).

```html
<div class="diff rounded-lg border border-slate-200 bg-white py-3 overflow-x-auto">
  <div class="ctx">export interface SessionStore {</div>
  <div class="del">  get(id: string): Session | undefined;</div>
  <div class="add">  get(id: string): Promise&lt;Session | null&gt;;</div>
  <div class="ctx">}</div>
</div>
```

Escape `<` and `>` inside diff lines (`&lt;`, `&gt;`) — a raw generic parameter silently disappears into the DOM.

The diff shows the **interface** — signatures, types, endpoint shapes — not imagined implementation bodies. When a body sketch is genuinely load-bearing (a state machine, a reducer), keep it ≤10 lines in a plain code block labelled *sketch*, outside the diff.

## Data model

The core entity shapes as real type declarations in one code block. When the relationships are genuinely graph-shaped, a Mermaid `classDiagram` or `erDiagram` may replace prose — that's the one case that earns the Mermaid import.

## Errors and edge cases

A compact table: seam · what can fail · how the failure surfaces in the types. Rows come from the session; a gap here is an open question, not a guess.

## Open questions

Amber callout box, one bullet per deferred decision. Deferred means listed here — never silently dropped.

## The gate (footer)

One quiet line: _"Review the diff, then approve in the terminal — implementation starts on your word. Signatures above are contracts."_

## Style

- Lean editorial, not corporate-dashboard: generous whitespace, one accent colour (indigo or emerald), amber for open questions, red only inside diffs.
- The review reads top-to-bottom in under two minutes. A card that needs a paragraph to be understood needs a better diff instead.
- Static HTML, no app code: the Tailwind CDN (plus Mermaid, only when imported) are the only scripts.
- When the plan touches code, prose speaks the `/codebase-design` vocabulary — module, interface, seam, adapter, depth, locality — and the project's own domain terms. No hedging, no throat-clearing.
