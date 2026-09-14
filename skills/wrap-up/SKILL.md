---
name: wrap-up
description: End-of-session capture. Use when Sumiya says "wrap up", "wrap-up", or "let's wrap this up": save the session's durable findings to memory and update the affected docs in each repo touched, so the next session starts from what this one learned. Not a commit command.
---

# wrap-up

Goal: nothing learned this session depends on this conversation surviving. Keep it short: record
findings, not a diary.

## 1. Collect what is worth keeping

Go back over the session and list the things a future session would otherwise re-discover the hard way:

- a claim that turned out wrong, and what is actually true
- a trap that cost time (environment, tooling, a silent failure mode)
- a decision Sumiya made, and his reason
- a behaviour of the system that the code does not make obvious

Skip anything already in code, git history, or an existing doc, and skip what only mattered today.

## 2. Memory

Save into the memory directory using its own rules (one fact per file, frontmatter, a pointer line
in `MEMORY.md`).

- **Update before you create.** If a memory already covers the topic, edit it. If a memory is now
  wrong, fix it or delete it; a stale memory is worse than none.
- Feedback and project memories carry **Why:** and **How to apply:**.
- If the project also keeps a tracked memory store (e.g. `docs/memory/`), cross-machine facts go
  there instead, because the local store does not travel between machines.

## 3. Docs, in every repo touched this session

The repo's `CLAUDE.md` says where its knowledge lives; follow that.

- **Session log / status file** (e.g. `docs/STATUS.md`): add a newest-first entry. Say what shipped
  or stayed open, and the findings. Correct any earlier line this session proved wrong.
- **Specs and design docs** that the change affected: API contracts, data model, plans, decision
  records. A doc describing the old behaviour is a bug.
- **READMEs and runbooks**, when a command, flag, port or setup step changed.
- **Instruction files and skills**: only rules that are now true in general. No history there.
- **Work tracker** (Jira etc.), if the project uses one: make each touched issue's status and last
  comment match reality, following the project's rules for transitions.

## 4. Report, then stop

List what changed as file paths plus one line each. Uncommitted repo changes are **not** committed or
pushed as part of wrap-up. Ask, one approval per action, per the global rules.
