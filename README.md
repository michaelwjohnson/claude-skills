# Claude Skills

Two skills for [Claude Code](https://claude.com/claude-code). They answer
different questions about the same piece of work, and they are meant to be run
separately.

| Skill | Asks | Catches |
|---|---|---|
| [`occams-razor`](occams-razor/) | Does every element earn its place? | Parts added for symmetry, completeness, or elegance rather than because a question requires them |
| [`check-claims`](check-claims/) | Is any of this false? | Statements that are fluent, plausible, and wrong |

## Why two skills and not one

A scope review reads structure. A claim check reads truth. They fail
independently, and passing one says nothing about the other:

> A false claim can carry a real question, duplicate nothing, and survive every
> structural pattern. Fluent and wrong looks exactly like fluent and right from
> inside the document.

Folding them together would mean one skill answering two questions — which is
itself one of the failure patterns `occams-razor` is looking for.

## occams-razor

Built on Einstein's formulation of the razor:

> "The supreme goal of all theory is to make the irreducible basic elements as
> simple and as few as possible **without having to surrender the adequate
> representation of a single datum of experience**."

**Both halves bind.** The first cuts; the second stops the cutting. This is not
minimalism — dropping something that carries a claim is the same failure as
adding something that doesn't, just in the other direction. The skill includes a
"What Is Not Excess" section for exactly this reason: stated requirements,
controls that isolate the variable under study, baselines, honest limitations,
and error handling on paths that can fail are never the thing to cut.

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

Both skills grew out of real errors. The worked example in `check-claims` is one
of them: a document asserted that a label's base rate was "the floor every model
must clear." True for accuracy. The metric was AUROC, whose chance floor is 0.5
regardless of prevalence. It survived a scope review, a vagueness sweep and a
style pass — none of which read for truth — and surfaced only when someone asked
what the metric meant.

## Install

Copy either directory into your skills folder:

```bash
git clone https://github.com/michaelwjohnson/claude-skills.git
cp -r claude-skills/occams-razor ~/.claude/skills/
cp -r claude-skills/check-claims ~/.claude/skills/
```

Then invoke with `/occams-razor` or `/check-claims`, or let Claude trigger them
from the `description` in each skill's frontmatter.

Project-scoped instead of user-scoped: use `.claude/skills/` in the repo.

## Using them together

Run the razor first, then the claim check — in that order, because cutting
changes which claims are still load-bearing, and because a cut leaves orphans
that are themselves often false.

## License

MIT
