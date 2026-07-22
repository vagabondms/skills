---
name: grill-to-plan
description: Close a grilling session by writing the settled plan and opening it in the browser as an HTML plan review. Use when a grilling session reaches shared understanding, or when the user asks to write up, finalize, or see the plan.
---

# Grill to Plan

Write the plan the session has earned and present it as a **plan review**: a self-contained HTML file opened in my browser, read the way a PR is reviewed — except the diff shows interfaces that don't exist yet.

If shared understanding hasn't been reached — decisions still open, branches unexplored — run a `/grilling` session first. The review renders decisions, not guesses.

## The plan is a contract

Write a real plan, not a loose summary of the conversation. When the plan touches code, it must be detailed enough that the interfaces are decided before implementation starts:

- **Modules** — which modules/files are created or changed, what each is responsible for, and what depends on what.
- **Interfaces** — for every new or changed public surface, the actual signatures: `interface`/`type` declarations, function signatures with parameter and return types, API endpoint specs (method, path, request/response schemas), or the equivalent in the project's language. No hand-waving like "a function that handles X".
- **Data model** — the shapes of the core entities involved and how they relate.
- **Errors and edge cases** — what can fail at each seam and how that failure is surfaced in the types (result types, thrown errors, status codes).
- **Build order** — the numbered implementation steps, each naming the modules it touches and the check that proves it landed.
- **Open questions** — anything deliberately deferred, listed explicitly rather than silently dropped.

Everything in the plan traces back to a decision we made in the session; a surface we never discussed belongs in Open questions, not in the diff.

Treat the signatures as **contracts**: implementation fills in bodies, but changing a signature means flagging it back to me first.

## Render the review

Write the review as a single self-contained HTML file — all CSS inline, no CDN, no scripts (see [PLAN-HTML.md](PLAN-HTML.md)) — in the OS temp directory so nothing lands in the repo. Resolve the temp dir from `$TMPDIR`, falling back to `/tmp` (or `%TEMP%` on Windows), and write to `<tmpdir>/plan-review-<slug>-<timestamp>.html` so each run gets a fresh file.

Then get it in front of me, in this order:

1. **System browser** — run `open <path>` (macOS), `xdg-open <path>` (Linux), `start <path>` (Windows). This works when the shell runs on my actual machine (Claude Code in a terminal).
2. **In-app viewer** — if the open command is missing, errors, or the shell is sandboxed away from my display (a Claude app / Cowork session, a remote container), attach or send the HTML file into the conversation instead, so it renders in the app's own viewer. The file is fully self-contained, so it renders styled there too.

In both cases tell me the absolute path. Never let the review die silently: if you can't confirm a browser opened, fall back to attaching the file — a plan review I never see is a plan review that didn't happen.

The centrepiece is the **change surface**: one card per module, its interface rendered as a git-style diff — current lines red, proposed lines green — so the plan is reviewed like a pull request. See [PLAN-HTML.md](PLAN-HTML.md) for the scaffold, card patterns, diff styling, and tone.

When the plan touches code, use the project's domain glossary vocabulary, respect any ADRs in the area you're touching, and speak the `/codebase-design` vocabulary for the architecture.

## The gate

Done when the review is in front of me — a browser tab or the in-app viewer — and every decision from the session appears in it. Then stop: ask me to review, and wait. Implementation starts only after I approve in the conversation — until then the review file is the only thing this session writes. When review turns up a change, amend the plan and re-render rather than defending it.

After approval, name my exits so I can type one: `/implement` to build it here, `/to-spec` to publish it to the issue tracker, `/to-tickets` to split it into tickets.
