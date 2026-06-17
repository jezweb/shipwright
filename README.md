# shipwright

The shared **operating method** for an autonomous agent that builds and ships software. The harness
already decides *which skill* fires; shipwright is the layer above it: the **loop you run**, the
**moves you reach for and how they chain**, and the **judgement that keeps the work honest**.

It exists for a fleet. Static rules ("don't fabricate, commit often, strip EXIF") travel between
agents and devices fine. The *active method* — the loop, the moves, the chains between them — does
not. shipwright is that method, packaged so every agent loads the same one and a new agent inherits
the whole approach on day one instead of a pile of separate tools.

## What it is, and isn't

- **Is:** doctrine — a loop, a move-set with composition chains, and a judgement layer. Picked up
  when you're about to build/fix/ship, or run end-to-end as a self-directed work loop.
- **Isn't:** a router (the harness already picks skills by description), a duplicate of your rules
  (it *references* them), or a checklist that replaces thinking.

## The three pillars

1. **The Loop** — read signals → pick the highest-value thing → branch → build → verify with proof →
   ship through the gates → post → improve. Plus the cadence judgement: tighten on real signal, ease
   on quiet, never manufacture busywork.
2. **The Move-set + chains** — which sibling to reach for (`decisions`, `share`, `brainstrust`,
   `fixer`, `honesty`, `walkabout`, `perfection`) and, crucially, how they feed each other.
3. **The Judgement layer** — verify-before-assert, panel-is-input-not-verdict, visual-proof, no
   fabricated specifics, correct-in-the-open, and the deploy-time-only failure modes — referencing
   the rules rather than copying them.

See [`skills/shipwright/SKILL.md`](skills/shipwright/SKILL.md) for the doctrine itself and
[`docs/SCOPE.md`](docs/SCOPE.md) for the design rationale and open questions.

## Siblings

shipwright is the method; these are the moves it orchestrates:
[`decisions`](https://github.com/jezweb/decisions) ·
[`share`](https://github.com/jezweb/share) ·
[`brainstrust`](https://github.com/jezweb/brainstrust) · `fixer` · `honesty` · `walkabout` ·
`perfection`.

## Status

v0.1.0 — early. The doctrine is real but young; it's meant to grow as more agents work against it and
contribute what they learn. If it doesn't earn its place, it's cheap to retire.

## Contributing

This is generic operating doctrine on purpose. **Keep it free of specific clients, projects, repos,
or private data** — examples must stay generic. Push fast-changing specifics down into the rules it
references; keep shipwright the slow-changing method.
