# learn-before-task

A [Claude skill](https://docs.claude.com/en/docs/agents-and-tools/agent-skills/overview) that turns every task into a learning opportunity.

Before you start a ticket, issue, bug, or feature, Claude gives you a short **"learn first" brief**:

- **The problem**: what is broken or missing, and why it matters
- **What you'll learn**: the 2–4 concepts the task really depends on, and *why* this task needs each one (tied to your actual code)
- **How we solve it**: you suggest an approach first, then Claude outlines the solution in a few steps, with the trade-offs
- Trusted resources only — no invented links
- A small timeboxed plan: read → read the code → predict-then-run experiment
- Self-check questions you answer in your own words before you start

When you finish, say **debrief**. Claude runs a short debrief and adds what you learned to your **learning log**.

## Why

It's easy to finish a task with AI help and learn nothing. This skill is built on a few ideas that make learning actually work:

- **Prior knowledge first** — Claude checks your learning log (or asks) and skips what you already know.
- **Few concepts, deeply** — only the ones the task can't be done without.
- **Concrete over abstract** — every concept is linked to a file, function, or failure case.
- **Prediction** — you guess the result of an experiment before running it.
- **Retrieval & explanation** — "why" questions before, a teach-back after.
- **Spaced review** — one self-check question brings back a concept from an earlier task.

## How it works

The brief comes in three short turns, so you think before you see the answer:

1. **The problem**, plus one question about your level (skipped if your log already answers it).
2. **What to learn and why**, plus one question: how would you approach it?
3. **How we solve it**, resources, plan, self-check questions, and the part of the system the task changes.

Say "full brief now" to get it in one go; the solution is then hidden in a collapsed block.

**Quick mode:** for medium tasks, say "quick brief" to get about 5 lines: the problem, the one key concept, one question, and one resource.

**Learning log:** after each debrief, Claude adds a row to `~/.claude/learning-log.md` (or your notes folder):

```
| Date | Task | Concepts | Self-rating (1–5) | Open question |
```

Concepts you rate 4–5 are skipped next time. Concepts you rate 1–2 get more depth. If Claude can't write files (for example, in the Claude app), it gives you the row to save yourself.

## Example

```
## Learn before: add retries to the payment webhook

## The problem
When the payment provider times out, `handle_payment()` fails once and the event is lost. Customers pay, but their orders stay "pending".

Quick check: have you worked with retries or idempotency before?
```

…then the concepts (idempotency, backoff with jitter), your approach, and the solution with its trade-off, resources, plan, and questions such as *"What happens if the same event arrives twice today?"* See the full example in [SKILL.md](SKILL.md).

## Install

Clone the repo:

```bash
git clone https://github.com/mnamnam/Claude-Skills.git
```

**Claude Code:** copy the skill into your personal skills folder:

```bash
cp -r Claude-Skills/Learn-before-task ~/.claude/skills/learn-before-task
```

Or copy it to `.claude/skills/learn-before-task` inside a repo to enable it for that project only. Restart Claude Code, then run `/learn-before-task` or just start a task.

**Claude.ai / Claude app:** zip the `Learn-before-task` folder from the cloned repo and upload it under Settings → Capabilities → Skills.

## Usage

Just start a task:

> I'm starting this issue: add rate limiting to the public API

Other phrases:

- "quick brief" — the 5-line version
- "full brief now" — all of it in one message
- "skip the brief" — when you already know the topic
- "debrief" — when the task is done

## Customize

Edit `SKILL.md` to fit you:

- **Concepts** — change the example list in "Pick concepts" to your domain (frontend, data, mobile, ML…).
- **Resources** — add the docs, books, or internal notes you trust in "Resources".
- **Time budget** — adjust the plan length in "Timeboxed plan".
- **Learning log** — change the log location in "Learning log".
