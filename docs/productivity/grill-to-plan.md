Quickstart:

```bash
npx skills add vagabondms/skills --skill=grill-to-plan
```

```bash
npx skills update grill-to-plan
```

[Source](https://github.com/vagabondms/skills/tree/main/skills/productivity/grill-to-plan)

## What it does

`grill-to-plan` closes a grilling session by writing the settled understanding up as a real plan and opening it in your browser as a **plan review** — a self-contained HTML page where every touched module's interface is rendered as a git-style diff: current signatures in red, proposed ones in green. You read the plan the way you read a pull request, except the diff shows code that doesn't exist yet. It renders only what the session actually decided — a surface the interview never visited can appear as an open question, never as a green line in the diff.

## When to reach for it

Type `/grill-to-plan`, or the agent reaches for it automatically the moment a grilling session lands on shared understanding — [grill-me](https://aihero.dev/skills-grill-me) ends by firing it. Reach for it directly when a design conversation has quietly settled and you want it pinned down as a reviewable contract before any code is written. For a durable spec published to the issue tracker instead of a browser page, use [to-spec](https://aihero.dev/skills-to-spec).

## The plan review

One file in your OS temp directory — nothing lands in the repo. Top to bottom: the decisions the grill resolved, the numbered build order, the **change surface** — one card per module with its interface diff — then the data model, how failures surface in the types at each seam, and the open questions. At the bottom sits the gate: implementation starts only when you approve in the terminal.

## Signatures are contracts

The diff is not an illustration — it is the contract. Implementation fills in bodies; changing a signature after approval means flagging it back to you first. And the red side of every diff is pulled from the code as it exists today, so what you approve is a true before/after of the interfaces.

## It's working if

- A grilling session's last act is a browser tab opening, not a wall of markdown in the terminal.
- Every red line in a diff exists in the codebase today; every green line traces to a decision from the session.
- The agent stops after the tab opens and waits for your approval before touching any code.

## Where it fits

`grill-to-plan` is the closing step of the grilling flow: [grilling](https://aihero.dev/skills-grilling) walks the decision tree, and this skill writes down where you landed. [grill-me](https://aihero.dev/skills-grill-me) fires it automatically as its deliverable. After approval it names your exits: [implement](https://aihero.dev/skills-implement) to build, [to-spec](https://aihero.dev/skills-to-spec) to publish, [to-tickets](https://aihero.dev/skills-to-tickets) to split. When you're unsure which flow fits, [ask-matt](https://aihero.dev/skills-ask-matt) routes you.
