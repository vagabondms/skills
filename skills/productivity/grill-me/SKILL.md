---
name: grill-me
description: A relentless interview to sharpen a plan or design.
disable-model-invocation: true
---

Run a `/grilling` session.

## The plan

Once we confirm shared understanding, write the plan down as an artifact — not a loose summary of the conversation. When the plan touches code, it must be detailed enough that the interfaces are decided before implementation starts:

- **Module boundaries** — which modules/files are created or changed, what each is responsible for, and what depends on what.
- **Public interfaces** — for every new or changed public surface, write the actual signatures: `interface`/`type` declarations, function signatures with parameter and return types, API endpoint specs (method, path, request/response schemas), or the equivalent in the project's language. No hand-waving like "a function that handles X".
- **Data model** — the shapes of the core entities involved and how they relate.
- **Errors and edge cases** — what can fail at each boundary and how that failure is surfaced in the types (result types, thrown errors, status codes).
- **Open questions** — anything deliberately deferred, listed explicitly rather than silently dropped.

Treat the signatures in the plan as contracts: implementation fills in bodies, but changing a signature means flagging it back to me first.
