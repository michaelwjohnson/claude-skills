# Claude Skills

Three skills for [Claude Code](https://claude.com/claude-code). Each reads a
different failure in the same piece of work, and they should be run separately.

| Skill | Asks | Catches |
|---|---|---|
| [`occams-razor`](skills/occams-razor/) | Does every element earn its place? | Parts added for symmetry, completeness, or elegance rather than because a question requires them |
| [`check-claims`](skills/check-claims/) | Is any of this false? | Statements that are fluent, plausible, and wrong |
| [`writing-review`](skills/writing-review/) | Does the writing work? | An unstated point, an order nobody can follow, claims with no evidence under them, and clutter that survived the draft |

## Why three skills and not one

A scope review reads structure. A claim check reads truth. A writing review
reads whether a reader can follow either. All three fail independently, and
passing one says nothing about the others:

> A false claim can carry a real question, duplicate nothing, and survive every
> structural pattern. Fluent and wrong looks exactly like fluent and right from
> inside the document.

Folding them together would mean one skill answering three questions — which is
itself one of the failure patterns `occams-razor` is looking for.

## Using them together

Cut first, then fix the writing, then verify what survived.

The razor goes first because cutting changes which claims are still
load-bearing, and because a cut leaves orphans that are themselves often false.
`writing-review` comes next, so you are shaping text that will survive rather
than polishing a section you are about to delete. `check-claims` goes last, on
the sentences that made it.

## Where they came from

The first two were written after the errors they catch, not before.

`check-claims` exists because a document asserted that a label's base rate was
"the floor every model must clear." True for accuracy. The metric was AUROC,
whose chance floor is 0.5 regardless of prevalence. It survived a scope review, a
vagueness sweep and a style pass — none of which read for truth — and surfaced
only when someone asked what the metric meant.

`occams-razor` gained its last two patterns the same way. A first pass cut two
arms from a design and *moved* them into a follow-up rather than removing them.
The arms were checked against the reason they left, never against the question
where they landed. That cut then stranded a hypothesis, a sample-size
calculation and a rollout plan. None of them named the removed arms, so
searching came back clean.

`writing-review` has a different origin: it merges two published sources — a
Campus Writing Program rubric aimed at argument, and Jeff Zych's notes on
Zinsser aimed at sentences. They disagree usefully. A draft can be immaculate
sentence by sentence and say nothing, so the skill checks the two levels
separately. Both sources are cited in the skill.

## occams-razor

Built on Einstein's formulation of the razor:

> "The supreme goal of all theory is to make the irreducible basic elements as
> simple and as few as possible **without having to surrender the adequate
> representation of a single datum of experience**."

**Both halves bind.** The first cuts; the second stops the cutting. This is not
minimalism — dropping something that carries a claim is the same failure as
adding something that doesn't, just in the other direction. So the skill carries a "What Is Not Excess" list. Stated requirements, controls
that isolate the variable under study, baselines, honest limitations, and error
handling on paths that can fail are never the thing to cut.

Eight failure patterns, including the two that are hardest to see from inside:

- **Relocation Without Rechecking** — an element cut from one section and moved
  to another rather than removed. It left the first place for a reason; nobody
  asked whether it fits the second.
- **Orphans of a Cut** — the justifications, controls, counts and fallbacks that
  served a removed element and stayed behind. They usually don't name it, so
  searching for the name won't find them.

## check-claims

Three questions per factual claim: where did it come from, what would make it
false, and can that be checked in a minute?

Its first pattern is **the neighbour's fact** — a statement true of a closely
related thing, applied to this one. These are the hardest to catch by reading,
because they are true, just not here.

## Install

**As plugins** — no clone, and `claude plugin update` keeps them current:

```bash
claude plugin marketplace add michaelwjohnson/claude-skills
claude plugin install occams-razor@claude-skills
claude plugin install check-claims@claude-skills
claude plugin install writing-review@claude-skills
```

**Or drop the files in directly** — each skill is a single file:

```bash
B=https://raw.githubusercontent.com/michaelwjohnson/claude-skills/main/skills
mkdir -p ~/.claude/skills/{occams-razor,check-claims,writing-review}
for s in occams-razor check-claims writing-review; do
  curl -fsSL $B/$s/SKILL.md -o ~/.claude/skills/$s/SKILL.md
done
```

Either way, invoke with `/occams-razor`, `/check-claims` or `/writing-review`, or
let Claude trigger them from the `description` in each skill's frontmatter.

Project-scoped instead of user-scoped: use `.claude/skills/` in the repo.

## License

MIT
