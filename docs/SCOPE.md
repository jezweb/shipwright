# shipwright — scope (draft for Jez to react to)

> The shared operating doctrine for autonomous Jezweb dev agents. Not a tool that
> does a task; the **method** an agent runs *while* doing tasks, and the map of how
> the tool-family composes. One doctrine, installed on every device, so a fleet of
> agents ships the same way.

## Why it exists

We now have a real family of build skills (`fixer`, `perfection`, `walkabout`,
`honesty`, `decisions`, `brainstrust`) and a standing autonomous work-loop. But the
*connective tissue* lives nowhere durable: how the loop runs, how the skills feed
each other, and the judgement disciplines that keep the work honest are scattered
across `~/.claude/rules/`, cron prompts, and one agent's head.

With many agents on many devices, that gap is the problem. Static rules ("don't
fabricate, strip EXIF, commit often") travel fine. The *active method* (the loop
you run, the moves you reach for, the chains between them) does not. shipwright is
that active layer: the operating manual every Jezweb agent loads, so behaviour is
consistent across the fleet and a new specialist inherits the whole method on day
one rather than a pile of tools.

## What it is NOT (boundaries)

- **Not a router.** The harness already routes: the model reads each skill's
  description and triggers and picks. shipwright must not reinvent that. Its value
  is doctrine, not dispatch.
- **Not a duplicate of the rules.** It *references* `~/.claude/rules/` and the
  shared claude-rules; it does not copy them. Rules = invariants; shipwright = the
  method that applies them.
- **Not a task tracker / project manager.** It's how you work, not what's on the list.
- **Not prescriptive micromanagement.** It encodes method + judgement, not a rigid
  checklist that removes thinking.

## Packaging + how it's consumed (one decision to confirm)

**Recommendation: a plugin, read as always-on doctrine, not trigger-gated.**

- A plugin gives versioning + per-device install via the marketplace muscle we
  already use, which is exactly right for "many agents, many devices".
- But unlike the sibling skills, you want this *active every loop*, not summoned by
  a keyword. So it should be loaded/oriented at **session start and each work-loop
  tick** (the loop cron points at it), the way an operating manual is always in
  effect, rather than fired on a trigger phrase.
- **Open fork for Jez:** (a) plugin-as-always-on-doctrine [recommended], vs
  (b) a normal triggered skill you invoke (`/shipwright`) to re-orient, vs
  (c) it lives in the synced `shared/claude-rules/` as a doctrine doc with no
  plugin. (a) wins on distribution + versioning; (c) is simplest but loses
  versioning and the marketplace install.

## The three pillars (the content shipwright encodes)

### 1. The Loop — the autonomous build-tick method

The shape of one tick (and the session it sits in):

```
read signals → pick the highest-value thing → branch → build → verify with proof
→ ship (staging → prod gates) → post the outcome → improve → repeat
```

Plus the cadence judgement (the part that's only in lived experience today):

- Tighten cadence on **real** signal (a reply, a new issue, a ship). Ease on
  **sustained quiet**. Never re-tighten in anticipation, and never manufacture
  thin work to look busy. (Quiet ticks that find nothing should stay light.)
- Pick a priority + a reason + ship. Don't stall, don't hedge, don't wait for
  steering. If genuinely nothing is actionable, do real fallback work (docs / KB /
  prep) rather than fabricate.

### 2. The Move-set + Composition chains

Task-shape to the right sibling skill, and crucially *how they chain*:

| When you need to… | Reach for | Chains with |
|---|---|---|
| get a **human's** call | `decisions` | often fed by `brainstrust` |
| **show a person a page** / richer input | `share` | sibling of `decisions` — a page on a link when a tap won't do (compare, mark up, a form) |
| get an **outside / other-model** read | `brainstrust` | feeds `decisions`; verify its output before acting |
| **prove a bug is fixed** | `fixer` | closes what `brainstrust`/review opened |
| check **comments/docs are true** | `honesty` | run before trusting a comment you'll act on |
| **onboard / demo** the app | `walkabout` | — |
| keep **driving the bar** | `perfection` | the *outer loop* that calls the others |

The chain is the point: e.g. *brainstrust panel → verify its claims in code →
decisions ask to the human → build → fixer prove-it → perfection keeps watch*.
That arc is what a new agent can't infer from six separate descriptions.

### 3. The Judgement layer (the disciplines)

References, not duplicates, the existing rules. The load-bearing ones:

- **Verify before asserting** (ground claims in the actual code / source; a
  model's confident claim is input, not truth).
- **The panel is input, not verdict** (synthesise; say when you disagree with all
  of them).
- **Visual proof before the announce** (show the change on real prod, don't just
  claim it).
- **No fabricated specifics** (numbers/dates/quotes need a source, a test, or
  lived experience).
- **Test pass ≠ surface live** (verify the change is actually reachable).
- **Commit to beat Syncthing** (commit after every substantive write).
- The deploy-time-only failure modes (hono-middleware-order, d1-no-raw-txn,
  cloudflare-deploy drift) as a named catalogue to check against.

## Distribution + relationship to existing mechanisms

- Installs per device as a plugin (versioned).
- **Above** `~/.claude/rules/` (which stay the synced invariants) and the
  virtual-team `shared/claude-rules/`. shipwright is the method that *applies*
  those rules; it links to them by name and does not copy their content (so they
  stay single-source).
- Composes with the umbrella learnings/memory mechanism: shipwright is the stable
  doctrine; learnings remain the evolving per-specialist log.

## Risks to design against

- **Doc-rot.** Doctrine that drifts from practice is worse than none. Mitigation:
  keep it method + chains (slow-changing); push the fast-changing specifics into
  the rules it references.
- **Over-prescription.** If it becomes a rigid checklist it removes the judgement
  it's meant to encode. Keep it principles + a decision map, not steps.
- **Overlap with the harness router.** Stay doctrine, never dispatch.

## Open questions for Jez

1. Packaging fork above: plugin-as-always-on-doctrine (recommended) vs triggered
   skill vs synced-rules-doc?
2. Should shipwright **own the work-loop cron prompt** (i.e. the loop directive
   becomes "run shipwright"), or just describe the loop the cron already runs?
3. Public repo (anonymise all examples) or private fleet-only?
4. Does it name `visualisation` (and any other future siblings) as known-but-unbuilt
   slots, so the family map is complete?

## If greenlit — build increments

1. This scope, agreed.
2. `SKILL.md` (the doctrine: loop + move-set + judgement), generic examples.
3. `plugin.json` + `marketplace.json`, MIT, install on one device, dogfood a loop
   tick against it.
4. Roll to the fleet; cross-link from `~/.claude/rules/README` and the sibling
   skills' "see also".
