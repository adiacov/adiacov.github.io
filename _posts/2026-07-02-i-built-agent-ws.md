---
layout: post
title: "I built agent-ws because my AI agents kept forgetting everything"
date: 2026-07-02
series: "Working with AI agents"
takeaways:
  - AI agents forget your project between sessions, and each tool keeps its memory to itself.
  - agent-ws sets up a few plain Markdown files that every agent reads, so you explain the project once.
  - The tool stays simple. It copies files and never reads them; the agent does the smart part.
  - Starting a new AI-ready project is a few commands instead of copying files by hand.
---

Every session with an AI coding agent starts the same way. The tool opens, and it knows nothing. It does not know what the project is, what we decided last week, or that the deploy script has a step you must never skip. So I explain. The next day I explain again, paste the same context, and only then get to the actual work.

The cost is small each time and constant, which is the worst kind of cost. You stop noticing it and just keep paying. And it gets worse when you use more than one agent, because the tools that do remember keep that memory to themselves. What one agent learned about my project, the next one cannot read. Switch tools and you start from zero again, except now part of your context is locked inside a store you cannot open or correct.

At some point the frustration won, and I built a small command line tool for it, [agent-ws](https://github.com/adiacov/agent-workspace).

## How it started

It did not start as a product idea. I keep a private repo where I plan everything I work on, and over time it grew a hand-made setup for working with agents: instruction files, current-state notes, a small adapter file for each tool I use. It worked well enough that every new project wanted the same thing, and the only way to give it that was to copy the files from the last project and edit them until they fit.

Around the third copy I admitted what I was really doing: maintaining a template system by hand. So I extracted it into a separate tool, deliberately independent of any single agent, and my planning repo became its first customer.

## Why one instructions file was not enough

The obvious answer to agent amnesia is to write one good instructions file and stop there. That instinct is right, and it is more right than it used to be, because `AGENTS.md` is an open standard now and most agents read it natively. One clear file genuinely helps.

But it helps less than it looks. One file mixes things that change at completely different speeds: what the project is changes maybe once a year, while what you are doing right now changes every hour, and both rot together in the same place. The file also says nothing about yesterday, so a new session still starts blind. And each file lives on its own. When you find a better way to write it in one project, your other projects do not get the improvement. You carry it into every repo by hand, and that is exactly the chore I was already tired of.

## What agent-ws does

You go into a repo and run `agent-ws init`. It asks for a profile and which agents you use, then drops a few plain Markdown files into the repo. `PROJECT.md` holds what the project is, the stable part. `STATE.md` holds what is true right now, the part that changes constantly. `WORKFLOWS.md` holds how you work. For software projects it adds `ENGINEERING.md` with build-and-test rules. The tool never reads your source code, and files that already exist are left alone.

All agents converge on one shared entrypoint, `AGENTS.md`. Most tools read it directly, the rest reach it through a one-line import. So the content is written once, every agent ends up following the same instructions, and adding another agent later means adding one more small pointer, not another copy of your rules.

The real behavior lives in `WORKFLOWS.md`, and its start-of-session part is the actual cure for the amnesia. It tells any agent to start by reading `STATE.md`, so the session begins with what is actually going on instead of a blank page. Then it says to look at the request before loading anything else, so a quick question does not drag in the whole project history. And because notes written by a past session can go stale, it sets one rule I care about: treat the notes as a hint, not the truth. If `STATE.md` says one thing and the code says another, the code wins. A sibling tool, [checkpoint](https://github.com/adiacov/checkpoint), saves a snapshot of the session automatically when it ends, so the next session can pick up interrupted work; the two tools fit together but neither needs the other.

Here is the part I like most. The tool itself does none of this work. There is no program running in the background, no database, no engine. Everything I just described is written as plain instructions in a Markdown file, and the agent reads them and follows them. Agents are already good at following clear written instructions, so the tool does not have to be smart. Its whole job is to put the right files in the right place and then stay out of the way. That one idea keeps the entire thing simple.

## The one hard problem

All the files agent-ws creates come from templates that ship with the tool. And templates evolve. I fix wording, add a better rule, improve the session workflow, and naturally I want those improvements to reach the projects I set up months ago.

That turns out to be the hardest problem in the whole tool. `WORKFLOWS.md` starts as a template copy, but it is also a file you edit, because your own project rules go into it. So two sides now own the same file. When a new template version arrives, overwriting your copy destroys your edits, and skipping the update means the file quietly goes stale in every repo.

That is a three-way merge, and `agent-ws sync` treats it as one. It keeps a snapshot of the template version each project last synced from and hands your file, that snapshot, and the new template to `git merge-file`, instead of a merge engine hand-written in shell. If you edited one section and the template changed another, the merge applies cleanly. If both sides touched the same lines, the tool refuses to guess. It leaves your file untouched, writes its attempt to a side file, and tells you to resolve it. A sync tool that quietly mangles a file you care about is worse than one that admits it could not do this one.

## What changed for me

The clearest win is starting a new project. That used to be the chore I described at the beginning: copy the setup files from the last project, edit them until they fit, repeat next time. Now it is a few commands. I run `agent-ws init`, answer two questions, and the new project is ready for AI work from the first session, with its memory files and workflow already in place.

Daily work changed in a quieter way. Sessions no longer start with me re-explaining the project; the agent reads the state file and continues. And when I switch agents in the middle of something, the next one picks up the same context, because none of it is locked inside one tool. It is all plain Markdown sitting in git. If agent-ws disappeared tomorrow, the files would still be there and would still work.

The tool is young, and so far it has mostly run on my own projects. If the problem sounds familiar, try it: [github.com/adiacov/agent-workspace](https://github.com/adiacov/agent-workspace). One install, one `init`, and you can judge the rest yourself.
