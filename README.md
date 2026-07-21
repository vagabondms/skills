# vagabondms' Skills

My personal agent skills for real engineering work — forked from [mattpocock/skills](https://github.com/mattpocock/skills) (MIT) and adapted to how I work.

The skills stay small, composable, and easy to adapt, per the original repo's philosophy. See the upstream README for the reasoning behind each skill.

## Changes from upstream

- **`grill-me`**: the session now ends by firing **`grill-to-plan`** — a plan review in the browser — instead of a conversational summary.
- **`grill-to-plan`** (new): closes a grilling session by writing the settled plan as a contract — modules, concrete `interface`/function signatures, data model, error cases, build order, open questions — and opening it in the local browser as a self-contained HTML **plan review**, each touched module's interface rendered as a git-style diff you approve before implementation starts.
- Renamed `setup-matt-pocock-skills` → `setup-vagabondms-skills`.
- Dropped the `personal/` and `deprecated/` buckets.

## Quickstart (30-second setup)

1. Run the skills.sh installer:

```bash
npx skills@latest add vagabondms/skills
```

2. Pick the skills you want, and which coding agents you want to install them on. **Make sure you select `/setup-vagabondms-skills`**.

3. Run `/setup-vagabondms-skills` in your agent. It will:
   - Ask you which issue tracker you want to use (GitHub, Linear, or local files)
   - Ask you what labels you apply to tickets when you triage them (`/triage` uses labels)
   - Ask you where you want to save any docs we create

4. Bam - you're ready to go.

## Install as a Claude Code plugin

Prefer a plug-and-play install you don't maintain by hand? These skills also ship as a native [Claude Code plugin](https://code.claude.com/docs/en/plugins). Instead of copying editable files into your repo, the plugin installs the whole skill set as a managed bundle that updates when I ship a new version — you subscribe rather than fork.

Inside Claude Code:

```
/plugin marketplace add vagabondms/skills
/plugin install vagabondms-skills@vagabondms
```

Or from your shell:

```bash
claude plugin marketplace add vagabondms/skills
claude plugin install vagabondms-skills@vagabondms
```

Then run `/setup-vagabondms-skills` once per repo, exactly as in the quickstart above.

Two ways to install, two philosophies:

- **[skills.sh](https://skills.sh/vagabondms/skills)** copies the skills into your project so you can hack on them and make them your own.
- **The plugin** keeps them as a read-only, always-current bundle you don't edit — best when you just want my set to work and follow along as it evolves.

> Using Codex or another agent? The [skills.sh installer](https://skills.sh/vagabondms/skills) already installs these skills into Codex and other Agent-Skills-standard harnesses today. A native Codex plugin is on the roadmap — see [`.agents/adr/0002-ship-as-a-claude-code-plugin.md`](./.agents/adr/0002-ship-as-a-claude-code-plugin.md).

## Why These Skills Exist

The philosophy behind these skills — the four failure modes of coding agents they fix — is explained in the [upstream README](https://github.com/mattpocock/skills#why-these-skills-exist).

## Reference

These split on one axis — who can invoke them. **User-invoked** skills are reachable only when you type them (e.g. `/grill-me`); their job is to orchestrate. **Model-invoked** skills can be invoked by you _or_ reached for automatically by the agent when the task fits; they hold the reusable discipline. A user-invoked skill may invoke model-invoked skills, but never another user-invoked one.

### Engineering

Skills I use daily for code work.

**User-invoked**

- **[ask-matt](./skills/engineering/ask-matt/SKILL.md)** — Ask which skill or flow fits your situation. A router over the user-invoked skills in this repo.
- **[grill-with-docs](./skills/engineering/grill-with-docs/SKILL.md)** — Grilling session that also builds your project's domain model, sharpening terminology and updating `CONTEXT.md` and ADRs inline.
- **[triage](./skills/engineering/triage/SKILL.md)** — Move issues through a state machine of triage roles.
- **[improve-codebase-architecture](./skills/engineering/improve-codebase-architecture/SKILL.md)** — Scan a codebase for deepening opportunities, present them as a visual HTML report, then grill through whichever one you pick.
- **[setup-vagabondms-skills](./skills/engineering/setup-vagabondms-skills/SKILL.md)** — Configure this repo for the engineering skills (issue tracker, triage labels, domain doc layout). Run once per repo before using the other engineering skills.
- **[to-spec](./skills/engineering/to-spec/SKILL.md)** — Turn the current conversation into a spec and publish it to the issue tracker. No interview — just synthesizes what you've already discussed.
- **[to-tickets](./skills/engineering/to-tickets/SKILL.md)** — Break any plan, spec, or conversation into a set of tracer-bullet tickets, each declaring its blocking edges — written as text in a local file, or as native blocking links on a real tracker.
- **[implement](./skills/engineering/implement/SKILL.md)** — Build the work described by a spec or set of tickets, driving `/tdd` at pre-agreed seams and closing out with `/code-review` before committing.
- **[wayfinder](./skills/engineering/wayfinder/SKILL.md)** — Plan a huge chunk of work, more than one agent session can hold, as a shared map of investigation tickets on the issue tracker — resolve them one at a time until the way to the destination is clear.

**Model-invoked**

- **[prototype](./skills/engineering/prototype/SKILL.md)** — Build a throwaway prototype to answer a design question — a runnable terminal app for state/logic questions, or several radically different UI variations toggleable from one route.
- **[diagnosing-bugs](./skills/engineering/diagnosing-bugs/SKILL.md)** — Disciplined diagnosis loop for hard bugs and performance regressions: reproduce → minimise → hypothesise → instrument → fix → regression-test.
- **[research](./skills/engineering/research/SKILL.md)** — Investigate a question against high-trust primary sources and capture the findings as a cited Markdown file in the repo, run as a background agent.
- **[tdd](./skills/engineering/tdd/SKILL.md)** — Test-driven development with a red-green-refactor loop. Builds features or fixes bugs one vertical slice at a time.
- **[domain-modeling](./skills/engineering/domain-modeling/SKILL.md)** — Actively build and sharpen a project's domain model — challenge terms against the glossary, stress-test with edge-case scenarios, and update `CONTEXT.md` and ADRs inline.
- **[codebase-design](./skills/engineering/codebase-design/SKILL.md)** — Shared discipline and vocabulary for designing deep modules: a lot of behaviour behind a small interface, placed at a clean seam, testable through that interface.
- **[code-review](./skills/engineering/code-review/SKILL.md)** — Two-axis review of the diff since a fixed point: **Standards** (does it follow the repo's coding standards, plus a Fowler smell baseline?) and **Spec** (does it faithfully implement the originating issue/PRD?), run as parallel sub-agents so neither pollutes the other.
- **[resolving-merge-conflicts](./skills/engineering/resolving-merge-conflicts/SKILL.md)** — Work through an in-progress git merge or rebase conflict hunk by hunk, resolving by intent traced to each side's primary source, then finish the operation — never `--abort`.

### Productivity

General workflow tools, not code-specific.

**User-invoked**

- **[grill-me](./skills/productivity/grill-me/SKILL.md)** — Get relentlessly interviewed about a plan or design until every branch of the decision tree is resolved.
- **[handoff](./skills/productivity/handoff/SKILL.md)** — Compact the current conversation into a handoff document so another agent can continue the work.
- **[teach](./skills/productivity/teach/SKILL.md)** — Teach the user a new skill or concept over multiple sessions, using the current directory as a stateful teaching workspace.
- **[writing-great-skills](./skills/productivity/writing-great-skills/SKILL.md)** — Reference for writing and editing skills well: the vocabulary and principles that make a skill predictable.

**Model-invoked**

- **[grilling](./skills/productivity/grilling/SKILL.md)** — Interview the user relentlessly about a plan, decision, or idea until every branch of the decision tree is resolved. The reusable loop behind `grill-me` and `grill-with-docs`.
- **[grill-to-plan](./skills/productivity/grill-to-plan/SKILL.md)** — Close a grilling session by writing the settled plan as a contract and opening it in the browser as an HTML plan review — interface diffs, build order, open questions — gated on your approval.
