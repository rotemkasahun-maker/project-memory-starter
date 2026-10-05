# Project Memory Starter

A tiny, tool-agnostic starter for AI-assisted coding projects.

Instead of relying on chat history, keep three pieces of durable project truth in your repo:

- **PROJECT.md** — what is actually working, the current milestone, next action, and blockers.
- **BUGS.md** — reproducible bugs, fixes, verification, and regression coverage.
- **HANDOFF.md** — what was completed, what was verified, what remains uncertain, and where the next session should start.

No app, subscription, vendor lock-in, or chat transcript storage.

## Setup

Create `PROJECT_MEMORY/`, copy the three templates into it, and tell your coding agent:

> Read PROJECT_MEMORY/PROJECT.md first. Read other memory files only when relevant. Do not store secrets or chat transcripts. Make a bounded change, verify it, record only durable learning, and leave a resumable handoff.

## Why this exists

AI coding sessions can lose working context, repeat already-solved bugs, or leave the next session unsure what is actually verified. These files make the handoff explicit and repo-local.

## Full kit

The paid **Project Memory + QA Lite** kit adds durable decisions, preventive lessons, regression contracts, release checks, a worked example, and a compact operating guide.

Paid product: https://rotemka.gumroad.com/l/project-memory-qa-lite
