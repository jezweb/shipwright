---
name: shipwright
description: >
  The operating method for autonomous build-and-ship work. Load it when you're about to build,
  code, refactor, fix, review, or ship a web app or service, or when running a self-directed work
  loop, and you want to work the way the rest of the toolkit expects: the loop to run, which sibling
  skill to reach for, how they chain, and the disciplines that keep the work honest. Not a router
  (the harness already picks skills) and not a task tracker — the method an agent runs WHILE doing
  the work. Triggers with 'shipwright', 'how should I work on this', 'start building', 'work loop',
  'ship this', 'autonomous dev loop', 'what's the right way to build this'.
triggers:
  - shipwright
  - how should I work on this
  - start building
  - run the work loop
  - autonomous dev loop
  - what's the right way to build this
  - ship this properly
user-invocable: true
argument-hint: "[optional: the task or 'loop']"
---

# shipwright

The shared operating method for an autonomous agent that builds and ships software. The harness
already decides *which skill* fires; shipwright is the layer above that: the **loop you run**, the
**moves you reach for and how they chain**, and the **judgement that keeps it honest**. It exists so
a fleet of agents across many machines ships the same way, and a new one inherits the whole method
on day one instead of a pile of separate tools.

Two ways it's used:
- **Pick it up for a task** — you're about to build/fix/ship something; read the move-set and the
  disciplines, then act.
- **Run it as the loop** — a self-directed work tick follows "The Loop" below, end to end.

It is **not** a router (don't re-derive what the harness does by description-matching), not a
duplicate of your rules (it *references* them), and not a checklist that replaces thinking.

## 1. The Loop

One work tick, and the session it lives in:

```
read the signals  →  pick the highest-value thing  →  branch
   →  build  →  verify with proof  →  ship (staging → prod gates)
   →  post the outcome  →  improve  →  repeat
```

- **Read the signals** before deciding: open work, new messages/issues, health of what's live, the
  breadcrumb you left last tick. And **re-read a thread or issue in the moment before you post into
  it or act on it** — async means a human may have answered since your last scan. Acting on stale
  state (e.g. posting a recommendation into a thread that was already decided) is the most common
  avoidable miss, and a fresh scan costs seconds.
- **Pick one thing, with a reason.** Highest value that's actually un-blocked. Don't stall, don't
  hedge into a survey of options, don't wait for steering you don't need.
- **Branch, build, verify.** Small commits. Prove the change does what you claim (see disciplines).
- **Ship through the gates.** Staging first; a behaviour change goes to the owner before prod.
- **Post the outcome**, then leave a breadcrumb for the next tick.

### Cadence (the part that's usually only lived, not written)

- **Tighten on real signal** (a reply, a new issue, a ship in flight). **Ease on sustained quiet.**
  Never re-tighten *in anticipation* of an answer, and never **manufacture thin work** to look busy.
- A genuinely quiet tick stays light: do real fallback work (docs, prep, grounding) or hold — do not
  fabricate activity. Fabricating busyness is the inverse failure of hedging, not its cure.

## 2. The Move-set, and how the moves chain

Reach for the sibling that fits the task shape. The **chain** matters more than the list:

| When you need to… | Reach for | Notes |
|---|---|---|
| get a **human's** decision | `decisions` | one clean ask, one tap; often fed by `brainstrust` |
| get an **outside / other-model** read | `brainstrust` | its output is *input, not verdict* — verify before acting |
| **prove a bug is fixed** (before/after) | `fixer` | closes what a review or panel opened |
| check **comments/docs match the code** | `honesty` | run before you trust a comment you'll act on |
| **onboard / demo** the app | `walkabout` | guided tour, ask-the-app, demo videos |
| keep **driving the bar** over time | `perfection` | the *outer loop* that keeps calling the others |

The arc, not the table, is the doctrine: e.g. *a `brainstrust` panel surfaces a concern → you verify
its claims against the real code → a `decisions` ask puts the call to the human → you build → `fixer`
proves it's fixed → `perfection` keeps watch.* A new agent can't infer that arc from six separate
descriptions; that's why it lives here.

## 3. The Judgement layer (the disciplines)

These reference your rules; they are not copied here. They are the difference between shipping and
shipping *well*:

- **Verify before asserting.** Ground every claim in the actual code/source. A model's confident
  statement (yours or a panel's) is a lead to check, not a fact.
- **A panel is input, not verdict.** Synthesise across opinions; say plainly when you disagree with
  all of them. Confident-in-chorus is still often wrong.
- **Visual proof before the announce.** Show the change working on real prod, don't just claim it.
- **Right channel for the load, and reviewer attention is the bottleneck.** An agent compresses months
  of work into days, so the scarce resource is the human reviewer's attention, not the work. Put
  substantive discussion, structured data, and full evidence on the durable threaded surface
  (issues / PRs); the chat channel gets a concise, human-readable *summary* with a link, never a dense
  wall. A glanceable summary plus visual proof (screenshot / GIF / short clip) conveys an outcome far
  more cheaply than paragraphs. Don't fragment context across reply-threads that lose the original.
- **No fabricated specifics.** A number, date, or quote needs a source, a test, or lived experience.
  Vague-but-true beats specific-but-invented.
- **Test pass ≠ surface live.** A green test doesn't mean the change is reachable by a user; verify
  the path.
- **Commit after every substantive write.** Durable work is committed work.
- **Know the deploy-time-only failure modes** (middleware-order, raw transactions on managed DBs,
  local-deploy drift, stale edge caches) and check against them — the test harness won't catch them.
- **Correct in the open.** If your recommendation flips on new evidence, say so and why. A clean
  correction beats a confident-but-stale recommendation, and saying it out loud is what keeps trust.

## Using shipwright

- Starting a build task: skim the move-set, pick the chain that fits, apply the disciplines as you go.
- Running the loop: follow §1 end to end each tick; let §2 pick your moves and §3 keep them honest.
- It composes with your invariant rules (which stay single-source and synced) and your evolving
  learnings log. shipwright is the stable method; those are the constraints and the diary.

> Keep this doctrine slow-changing. Push fast-moving specifics down into the rules it references, so
> shipwright stays the method and doesn't rot into a changelog.
