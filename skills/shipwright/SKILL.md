---
name: shipwright
description: >
  The operating method for autonomous build-and-ship work. Load it when you're about to build,
  code, refactor, fix, review, or ship a web app or service, or when running a self-directed work
  loop, and you want to work the way the rest of the toolkit expects: the loop to run, which sibling
  skill to invoke, how they chain, and the disciplines that keep the work honest. Not a router
  (the harness already picks skills) and not a task tracker — the method an agent runs WHILE doing
  the work. Triggers with 'shipwright', 'how should I work on this', 'start building', 'work loop',
  'ship this', 'autonomous dev loop', 'what's the right way to build this'.
user-invocable: true
argument-hint: "[optional: the task or 'loop']"
---

# shipwright

The shared operating method for an autonomous agent that builds and ships software. The harness
already decides *which skill* fires; shipwright is the layer above that: the **loop you run**, the
**skills you invoke and how they chain**, and the **judgement that keeps it honest**. It exists so
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

- **Tighten on real signal** (a reply, a new issue, a ship in flight). Stay tight while there's work —
  and there usually is.
- **A quiet tick is a discovery tick, not an idle one.** When nothing's obviously in front of you,
  don't ease into waiting or "hold" — **fan out read-only discovery agents in parallel**, one per
  surface (open PRs, the issue backlog, the team chats, the plan-of-record), to surface work that
  isn't on your radar yet. There is almost always real undone work; the skill is *finding it and
  shipping it*, not waiting for it to arrive. What the sweep returns becomes your to-do list — then you
  go do it.
- **Never re-tighten in anticipation** of an answer, and **never manufacture thin work.** The
  discipline cuts both ways: discovery surfaces *real* work, it doesn't invent it — and fabricating
  busyness is as wrong as idling. The cure for a quiet tick is *find and do*, never *hold*.

## 2. The Move-set, and how the moves chain

Each row is an **installed sibling skill**. To use one, **invoke it by name** (`/decisions`,
`/brainstrust`, and so on) so its full method loads, then follow it. The row is a one-line signpost,
never the skill itself — improvising the skill's job from the row instead of loading it is the mistake
this section exists to prevent.

Gate yourself first. You answer most questions by reading the code or taking the obvious default.
Invoke a skill only when the task earns it: a panel for a trivial call, or asking a human what you
could look up, costs more than it saves.

Match the skill to the task — the **chain** matters more than any single move:

| To do this… | Invoke | Notes |
|---|---|---|
| get a **human's** decision | `decisions` | one clean ask, one tap; often fed by `brainstrust` |
| **show a person a page** / get richer-than-a-tap input | `share` | a purpose-built web page on one link, structured answer back: compare options side by side, approve or edit a draft, mark up a mockup, fill a form. Sibling of `decisions` — `decisions` is one tap over chat, `share` is when the call needs a *surface* |
| get an **outside / other-model** read | `brainstrust` | its output is *input, not verdict* — verify before acting (the `brainstrust` skill, not the older `dev-tools:brains-trust`) |
| **prove a change works / review before ship** | `verify` · `run` · `code-review` / `security-review` | the §1 "verify with proof" + "ship through gates" steps — don't hand-roll them |
| **prove a bug is fixed** (before/after evidence) | `fixer` | closes what a review or panel opened |
| check **comments/docs match the code** | `honesty` | run before you trust a comment you'll act on |
| **onboard / demo** the app | `walkabout` | guided tour, ask-the-app, demo videos |
| keep **driving the bar** over time | `perfection` | the *outer loop* that keeps calling the others |

The arc, not the table, is the doctrine. The full version: *a `brainstrust` panel surfaces a concern →
you verify its claims against the real code → a `decisions` ask (or a `share` page when the call needs a
visual — two options to tap between, a mockup to mark up) puts it to the human → you build →
`fixer` proves it's fixed → `perfection` keeps watch.* But most ticks are leaner and need no fancy
moves — e.g. *read signals → pick a dependency security advisory → verify the fixed version is real and
the bump isn't breaking → build → `verify`/`run` it works (and be honest if a test can't run locally) →
ship the PR → breadcrumb*: no human, no panel, the disciplines alone carry the tick. A new agent can't
infer either arc from the separate skill descriptions; that's why it lives here. Don't invoke moves a
tick doesn't need.

## 3. The Judgement layer (the disciplines)

These reference your rules; they are not copied here. They are the difference between shipping and
shipping *well*:

- **Do the work; don't report the backlog.** This is a *doing* method — its output is shipped change,
  not a status of what's undone. Your job is to *clear* the pile, not to maintain a visible tally of
  it. A list of everything outstanding, handed to the human, transfers the burden back onto them and
  reads as nagging, not progress. So: find work → *do it*. What you surface to the human is a **ship**
  (here's what's now done, with proof) or a **genuine their-call decision** (a real gate, a credential
  only they hold, a preference only they can set) — never a catalogue of pending items. When a
  discovery sweep returns a backlog, that backlog is *your* to-do list, not their inbox.
- **Verify before asserting.** Ground every claim in the actual code/source. A model's confident
  statement (yours or a panel's) is a lead to check, not a fact. This is doubly true for anything you
  can't run end-to-end: "sound by construction" is a *hypothesis*, not a verification — ship such a
  change explicitly UNVERIFIED and route the live check to whoever (or whatever loop) can actually run
  it, rather than dressing a confident guess up as done.
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
- **Assert on the accessibility tree, not the pixels.** Locate elements and check state via the a11y
  tree (text, roles, a page snapshot) coupled to *user intent*, not DOM selectors — it survives
  re-renders and reflects what the user actually gets. Reserve screenshots for what only human
  perception catches: layout, spacing, colour encoding. Cheaper and more robust than screenshot-diffing.
- **Make claims PASS/FAIL, and verify against intent — not aesthetics.** "The out-of-spec product's
  badge renders red" is checkable; "looks right" isn't. Checking against the *spec* (not whether it's
  pretty) is what catches the domain-correctness bugs code review misses — wrong data in a chart, an
  inverted colour encoding, a palette that overflows the category count, a control that's unreachable.
- **Verify by inspection, never by exit code.** Actually look at the rendered result — read the page
  snapshot, or extract the frame and view it; a green build or a 200 is not proof the user sees the
  right thing. Don't judge from a single state, either (see "cover the states").
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

## 5. Wiring check (run when you pick shipwright up in a repo)

A skill auto-loads by its description — but an agent reads the project's `CLAUDE.md`
(or `AGENTS.md`) *first*, and if that brief never mentions the toolkit, a fresh agent may
never reach for it. So the first time you run shipwright in a repo — especially as the loop —
verify the repo is wired:

- **Does `CLAUDE.md` / `AGENTS.md` reference the `shipwright` method and the current sibling
  set?** If it's missing, or has *drifted* (names a retired skill, or omits one that now
  exists), **offer to add or update a short Operating-method block.** `CLAUDE.md` is
  human-owned — propose the edit, don't make it silently.
- **Are the siblings actually installed?** A pointer to a skill the repo can't load is a dead
  end; flag any that aren't available.

Running this check on *every* pickup is what stops the block from rotting: the wiring stays
current because shipwright reconciles it each time, rather than every repo's `CLAUDE.md`
drifting the moment the toolkit changes. (That self-heal is the whole point — it's what makes
listing the skills safe.)

Make the block **directive, not just a pointer**. A names-only list ("these skills exist, see
shipwright") fixes *discovery* — an agent that didn't know the toolkit existed. But the more common
failure is the **in-flow miss**: an agent that *knows* the skill exists still improvises past it in
momentum (e.g. dumps a wall of questions at a person instead of invoking `decisions`). A pointer
doesn't catch that; a **trigger → skill** mapping in the always-loaded brief has a chance to, because
it fires at the decision point. So the block maps trigger → skill inline, and still defers to this
skill for the *chains* and depth (single source). The per-pickup wiring-check keeps the trigger list
current, so the inline list self-heals rather than rotting. The block to add:

> **Operating method — shipwright.** Build and ship via the `shipwright` skill (load it for the
> loop + how the moves chain). **When one of these triggers comes up, invoke the named skill
> instead of improvising its job** — invoking loads the skill's full method; the line here is the
> trigger, not a substitute:
> - about to ask a person a decision → `decisions` (one clean ask, never a wall of questions)
> - want a page / richer-than-a-tap answer from a person → `share`
> - want an outside / other-model read → `brainstrust` (its output is input, not verdict)
> - proving a bug is fixed (before/after evidence) → `fixer`
> - checking comments/docs actually match the code → `honesty`
> - onboarding or demoing the app → `walkabout`
> - keeping the quality bar driven over time → `perfection`

(Adjust the list to the siblings actually installed — that's what the wiring-check reconciles.)

## Using shipwright

- Starting a build task: skim the move-set, pick the chain that fits, apply the disciplines as you go.
- Running the loop: follow §1 end to end each tick; let §2 pick your moves and §3 keep them honest.
- It composes with your invariant rules (which stay single-source and synced) and your evolving
  learnings log. shipwright is the stable method; those are the constraints and the diary.

> Keep this doctrine slow-changing. Push fast-moving specifics down into the rules it references, so
> shipwright stays the method and doesn't rot into a changelog.
