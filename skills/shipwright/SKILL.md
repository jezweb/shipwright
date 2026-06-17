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

To run this as a recurring self-directed loop (not just once), schedule it with the `loop` skill
(session-local) or `schedule` skill (cloud cron); each firing re-enters this method. Tighten or ease
the interval per the cadence rule below.

### Cadence (the part that's usually only lived, not written)

- **Tighten on real signal** (a reply, a new issue, a ship in flight). **Ease on sustained quiet.**
  Never re-tighten *in anticipation* of an answer, and never **manufacture thin work** to look busy.
- A genuinely quiet tick stays light: do real fallback work (docs, prep, grounding) or hold — do not
  fabricate activity. Fabricating busyness is the inverse failure of hedging, not its cure.

## 2. The Move-set, and how the moves chain

Each row below is an **installed sibling skill — invoke it by name** (the harness lists them; trigger
the one that fits). First gate yourself, though: most questions you resolve by reading the code or
picking the obvious default. Reach for a skill only when the task genuinely earns it — a panel for a
trivial call, or a human decision you could've answered yourself, wastes more than it saves.

Reach for the sibling that fits the task shape. The **chain** matters more than the list:

| When you need to… | Reach for | Notes |
|---|---|---|
| get a **human's** decision | `decisions` | one clean ask, one tap; often fed by `brainstrust` |
| get an **outside / other-model** read | `brainstrust` | its output is *input, not verdict* — verify before acting (the `brainstrust` skill, not the older `dev-tools:brains-trust`) |
| **prove a change works / review before ship** | `verify` · `run` · `code-review` / `security-review` | the §1 "verify with proof" + "ship through gates" steps — don't hand-roll them |
| **prove a bug is fixed** (before/after evidence) | `fixer` | closes what a review or panel opened |
| check **comments/docs match the code** | `honesty` | run before you trust a comment you'll act on |
| **onboard / demo** the app | `walkabout` | guided tour, ask-the-app, demo videos |
| keep **driving the bar** over time | `perfection` | the *outer loop* that keeps calling the others |

The arc, not the table, is the doctrine. The full version: *a `brainstrust` panel surfaces a concern →
you verify its claims against the real code → a `decisions` ask puts the call to the human → you build →
`fixer` proves it's fixed → `perfection` keeps watch.* But most ticks are leaner and need no fancy
moves — e.g. *read signals → pick a dependency security advisory → verify the fixed version is real and
the bump isn't breaking → build → `verify`/`run` it works (and be honest if a test can't run locally) →
ship the PR → breadcrumb*: no human, no panel, the disciplines alone carry the tick. A new agent can't
infer either arc from the separate skill descriptions; that's why it lives here. Don't over-reach for
moves a tick doesn't need.

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

## 4. When the work is UI

A user-facing change adds checks the back-end disciplines don't cover. These are **inline — don't reach
for a separate design tool**; they're the floor:

- **Look at it rendered, not just built.** A UI change isn't done until you've seen it on the *running*
  app (the visual-proof discipline), at the widths that matter — at least a narrow/mobile and a
  wide/desktop. Watch for overflow, broken wrapping, and a layout that only holds at your window size.
- **Match the app's existing design language.** Use its design tokens (colour, spacing, type scale) and
  its existing components and patterns — never hardcode a colour or introduce a new aesthetic.
  Consistency beats novelty: a view that looks "designed by someone else" is a regression even if it's
  pretty.
- **Cover the states, not just the happy path.** Empty (no data yet), loading, error, and the first-run
  case — not only the seeded-data screenshot. Most UI bugs live in the states you didn't render.
- **Hierarchy and rhythm.** One clear primary action per view; consistent spacing; aligned edges; the
  most important thing biggest/first. If everything shouts, nothing does.
- **Accessibility floor.** Real semantic elements (a button is a `<button>`), labelled inputs,
  sufficient contrast, keyboard-reachable. Baseline, not optional polish.
- **The path works end to end.** Sensible defaults, no dead-ends, the change is reachable and obvious
  (ties to "test pass ≠ surface live"). Walk it once as the user would.

## Using shipwright

- Starting a build task: skim the move-set, pick the chain that fits, apply the disciplines as you go.
- Running the loop: follow §1 end to end each tick; let §2 pick your moves and §3 keep them honest.
- It composes with your invariant rules (which stay single-source and synced) and your evolving
  learnings log. shipwright is the stable method; those are the constraints and the diary.

> Keep this doctrine slow-changing. Push fast-moving specifics down into the rules it references, so
> shipwright stays the method and doesn't rot into a changelog.
