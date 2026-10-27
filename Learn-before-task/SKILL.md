---
name: learn-before-task
description: "Before starting a new task (ticket, issue, spec, PR, bug, or feature), give the user a short 'learn first' brief: the problem the task solves and why it matters, the 2–4 concepts the task really depends on, why this task needs each one, how the problem will be solved, trusted resources, a small timeboxed study plan, and self-check questions. After the task, run a short debrief and update the user's learning log. Use this when the user pastes a ticket or issue, says they are starting something new ('I'm starting…', 'new ticket', 'picking up this issue'), asks for a 'learn first' or 'quick' brief, or says they want to learn while working. Also use it when the user says the task is done, the PR is merged, or asks for a 'debrief'. Do not use it for small edits, quick questions, or routine work the user did not frame as a new task."
---

# Learn before the task

The goal is real understanding, not a reading list. The user should finish the task able to explain *why* the solution works, not only *that* it works. Give a short brief before any plan or code, and a short debrief after.

## When to use

- The user starts a new task: a ticket, issue, spec, PR review, bug, or feature.
- Run it BEFORE writing a plan or code. If they ask only for the plan, give the brief first, then offer the plan.
- Skip it if the user says they already know the topic, or asks to skip. For tiny tasks (a typo, a rename), skip it unless asked.
- Use **quick mode** (see below) for medium tasks, or when the user says "quick brief".

## Language

Reply in the user's language. Keep technical names (functions, files, commands, libraries) as they are, in backticks. Use short, plain sentences.

## Learning log

The log lets the brief build on what the user has already learned.

- Location: the notes folder the user named, otherwise `~/.claude/learning-log.md`.
- Format: one table row per task.

```
| Date | Task | Concepts | Self-rating (1–5) | Open question |
```

- Before the brief, read the log if it exists. Skip concepts rated 4–5. Go deeper on concepts rated 1–2.
- If you cannot read or write files, ask the user about their level instead (step 3), and give the log row in the debrief for them to save.

## Flow

The brief has three short turns, so the user thinks before seeing the answer.

- **Turn 1:** the problem, plus one question about the user's level (skip the question if the log already answers it).
- **Turn 2:** what to learn and why, plus one question: "How would you approach this? One or two lines."
- **Turn 3:** how we solve it, resources, plan, check yourself, and system part.

If the user says "full brief now" or doesn't want to answer, send turns 2 and 3 together and put "How we solve it" inside a `<details>` block so they can think before opening it.

## Steps

1. **Understand the task.** Read the ticket or issue, any linked notes, and the relevant code if you can reach it (use connectors or files when available). Read the learning log. Do not change any code.

2. **State the problem.** In two or three sentences, say what is broken or missing, who or what it affects, and why it matters. Name a concrete failure case or symptom when there is one. If the ticket only describes a solution ("add retries"), dig out the problem behind it ("webhooks are lost when the provider times out"). Understanding the problem first is what makes the concepts stick.

3. **Check what the user already knows.** Use the log first. If it doesn't cover the topic, ask one quick question about the main topic. Build the brief on what you learn: skip what they know, go deeper where they are unsure.

4. **Pick concepts.** Choose 2–4 load-bearing concepts: the ideas the task cannot be done well without, not every topic it touches. Prefer fundamentals that transfer to other work (for example: concurrency, error handling, data consistency, caching, timeouts, state management, testing strategy, security boundaries) over tool trivia.

5. **Tie each concept to this task.** In one or two sentences, say why this task needs it. Point to the real code, file, function, or failure case. A concept with no link to the task does not belong in the brief.

6. **Outline how we solve it.** Do this only after the user has shared their approach (see Flow). Give the approach in 3–5 short steps, at the level of ideas, not code. Link each step to the concept it uses, and name the main trade-off or the option you rejected and why. If the user's idea was different but valid, say so. Keep it short: the full plan comes after the brief.

7. **Resources.** Give trusted resources only. Prefer official docs, well-known books, and respected engineering blogs. Never invent a title, author, or link. If you are not sure a resource exists, name the topic and the official docs site instead. Search to verify when a search tool is available. One or two resources per concept is enough.

8. **Timeboxed plan.** Scale it to the task (about 30 minutes for a small task, 60–90 for a large one):
   - Read: the key resource for each concept.
   - Read the code: the exact files or functions where the concept shows up.
   - Experiment: a small throwaway program or test that shows the concept. Ask the user to **predict the result before running it**, then compare. The gap between prediction and result is where learning happens.

9. **Check yourself.** Give 3 self-check questions. Ask "why" and "what happens if" questions, not definitions. Example: "What happens to the request if the downstream call never answers?" If the log has a concept from an earlier task that relates to this one, make one question about it (spaced review). The user answers them in their own words before starting. If they can't, they go back to the resource. Do not give the answers in the brief.

10. **System part.** In one sentence, name the part of the system the task changes and what depends on it.

## Format

- Short: one heading per part, about 30 lines in total across the three turns.
- Order: the problem, what to learn and why, how we solve it, resources, plan, check yourself, system part.
- No code changes and no implementation in the brief.
- End turn 3 with: "Want the plan now? Say *debrief* when you're done."

## Quick mode

For medium tasks, or when asked for a "quick brief", send one message of about 5 lines:

- The problem, in one sentence.
- The one concept that matters most, and why this task needs it.
- One self-check question.
- One trusted resource.

Then offer the full brief or the plan.

## After the task: debrief

When the user says the task is done, the PR is merged, or asks for a "debrief", offer a 2-minute debrief. Ask:

- Explain the problem and how you solved it in two or three sentences, as if to a teammate.
- Which concept mattered most, and where exactly did it show up?
- What surprised you, or what was different from your first prediction?
- What would you do differently next time?
- One thing to look up later.

Correct any misunderstanding gently and precisely. Then ask the user to rate each concept 1–5, and add a row to the learning log (or give the row for them to save). Use "one thing to look up later" as the open question.

## Teaching principles

- Let the user do the thinking. Ask before telling; give hints before answers.
- Prefer fewer concepts understood deeply over many skimmed.
- Make it concrete: always tie ideas back to their code.
- Be honest about uncertainty. Say "I'm not sure" rather than guess a resource or a fact.

## Example shape

Turn 1:
```
## Learn before: add retries to the payment webhook

## The problem
When the payment provider times out, `handle_payment()` fails once and the event is lost. Customers pay, but their orders stay "pending".

Quick check: have you worked with retries or idempotency before?
```

Turn 2:
```
## What to learn
1. Idempotency
   Why: a retry can deliver the same webhook twice. `handle_payment()` in `webhooks.py` would charge twice.
2. Exponential backoff with jitter
   Why: if the provider is down, fixed-interval retries from every worker hit it at the same moment.

How would you approach this? One or two lines.
```

Turn 3:
```
## How we solve it
1. Retry failed webhooks from the queue, not inline. Uses: backoff.
2. Wait longer after each failure, with random jitter. Uses: backoff with jitter.
3. Store each event ID and skip ones already processed. Uses: idempotency.
Trade-off: we accept a short delay instead of instant processing. We reject inline retries because they block the worker.

## Resources
- Stripe docs: idempotent requests
- AWS Architecture Blog: exponential backoff and jitter

## Plan (60 min)
1. Read both resources (20 min).
2. Read `handle_payment()` and the queue config (20 min).
3. Write a tiny script that retries a failing call. Predict the timing first, then run it (20 min).

## Check yourself
- What happens if the same event arrives twice today?
- Why does jitter help when many clients retry at once?
- Which errors should never be retried?

## System part
The payment ingest path: webhook handler → queue → ledger writes.

Want the plan now? Say *debrief* when you're done.
```
