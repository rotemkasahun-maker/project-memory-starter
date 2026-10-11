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


## Example: make a bug fix survive the handoff

**Fictional example:** an app stores a date-only deadline as midnight UTC, then formats it as a timestamp in the user's timezone. A deadline intended to mean **July 1, 2026** can appear as **June 30** in a timezone west of UTC. That may be correct for a real instant; it is a bug only when the product requirement is a calendar date. Record which meaning the feature requires before changing the code.

A compact bug entry should make the distinction testable:

- **Reproduction:** date-only value `2026-07-01`; display timezone `America/Los_Angeles`.
- **Expected:** the date-only deadline still displays as July 1.
- **Observed:** the conversion displays June 30.
- **Regression check:** assert the date-only contract separately from timestamp conversion. Keep one test for each behavior so a fix for one cannot silently break the other.

Then leave the next session a handoff it can act on:

- **Current truth:** which representation the code uses and which requirement it must satisfy.
- **Verified:** the exact test command and result that passed.
- **Still unknown:** for example, whether a database round-trip preserves the date.
- **Next action:** test that round-trip before describing the fix as complete.

The useful boundary is simple: a test that passed is evidence; a deployment or persistence path you did not test stays unverified. Do not paste secrets or private chat transcripts into project memory.

The free starter above covers current truth, bugs, and handoffs. I made the $19 [Project Memory + QA Lite kit](https://rotemka.gumroad.com/l/project-memory-qa-lite?utm_source=github&utm_medium=organic_repo&utm_campaign=pmqa_first_dollar_20261011&utm_content=starter_readme_handoff_example) for builders who also want decision and lesson logs, regression contracts, release checks, a worked example, and a compact guide. It is a manual Markdown convention, not automatic AI memory. I'm the maker.
