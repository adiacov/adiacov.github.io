# Workflows

This file is the primary workflow authority for this repository.

Agent-specific adapter files should only bootstrap the agent into this file, not duplicate its rules. If instructions appear to conflict, prefer the more specific instruction for the current repository/task, and ask the user when the correct priority is unclear.

## Start of session

Before continuing meaningful work:

1. Read the agent adapter/instruction file for the current tool if present and not already read.
2. Read `COLLABORATION.md` if present.
3. Read `MEMORY.md` if present, then read project memory in the order it defines.
4. Reconcile memory with reality before continuing work:

   - check `sessions/pending/` for raw checkpoint files, ignoring placeholder files such as `.gitkeep`;
   - inspect `git status` and recent commits;
   - inspect project files mentioned by memory, such as briefs, plans, READMEs, or implementation notes;
   - inspect relevant local/external task state when current work mentions tasks;
   - compare these facts with project memory.
5. Treat memory as curated context, not unquestionable truth. Repository state, task systems, and current project files take precedence when they disagree with memory.
6. If durable memory is stale or contradicted by repo/task reality, update memory or project docs before continuing normal work.
7. If the task is unclear after reconciliation, ask what we are working on.
8. Read project scope files relevant to the current task if present, such as `BRIEF.md` or `projects/<project-name>/BRIEF.md`.
9. For coding or implementation work, read and follow `ENGINEERING.md` if present.

## Collaboration style

Follow `COLLABORATION.md` when present. In general:

- work as a collaborative partner, not an autonomous task executor;
- prefer dialogue over assumptions when requirements, tradeoffs, priorities, or constraints are unclear;
- for non-trivial work, discuss the approach before implementation;
- present one major decision at a time rather than large batches of options;
- do not rush into implementation when understanding is incomplete;
- challenge assumptions when evidence suggests a better approach;
- keep communication concise and focused;
- when multiple reasonable approaches exist, explain the tradeoffs and recommend one.

## Pending checkpoint handling

Pending checkpoint files are raw recovery evidence, not curated memory.

If pending checkpoints exist:

1. review them before continuing normal work;
2. extract only durable goals, decisions, current state, next actions, blockers, and important realizations;
3. update project memory/docs as appropriate;
4. move processed checkpoint files to `sessions/archive/`.

If a checkpoint contains only information already reflected in memory/project files, archive it without duplicating content.

Do not blindly copy raw conversation into durable memory.

Manual durable-memory updates remain preferred when practical.

A global opt-in checkpoint extension is enabled for this repo via `.pi/checkpoint.json`.
It automatically writes raw shutdown checkpoints to `sessions/pending/` as a safety net.

## Project exploration and execution workflow

Use this repository as the strategy, memory, and reflection workspace.
Build actual tools in separate dedicated project repositories.

Stay in this repository when discussing:

- long-term goals and direction;
- project discovery and idea evaluation;
- problem framing and target users;
- MVP scope and non-goals;
- reflection after implementation work.

Switch to a dedicated project repository when there is enough clarity on:

- provisional project name;
- problem statement;
- intended user;
- MVP scope;
- first implementation task.

Before switching, create a project brief in this repository, for example:

```text
projects/<project-name>/BRIEF.md
```

The brief should preserve:

- why the project exists;
- how the idea was conceived;
- target user and usage scenario;
- MVP scope;
- non-goals;
- example usage;
- open questions;
- current decisions;
- next steps.

Then copy or adapt the brief into the dedicated project repository's own memory/instruction files so future coding agents can continue without relying on chat history.

Recommended loop:

```text
life-os:
  explore idea
  define project brief
  decide MVP

tool repo:
  implement small version
  test/demo
  update project state

life-os:
  reflect on what was learned
  decide whether to continue, pivot, or stop
```

## Implementation workflow

When coding or editing files:

1. understand the request and affected area;
2. inspect existing files before proposing changes;
3. for non-trivial work, present a short plan;
4. explain intended changes briefly when useful;
5. make minimal, precise edits;
6. preserve existing content unless explicitly asked to reorganize it;
7. run relevant checks when possible;
8. summarize changed files, verification performed, and next steps.
