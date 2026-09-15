---
name: check-claims
description: 'Use before sending, submitting, or publishing anything containing factual claims — a proposal, report, spec, README, analysis, review, or answer to a question. Catches statements that are fluent and wrong: numbers whose source nobody can name, thresholds attached to the wrong metric, facts that are true of a neighbouring concept, and figures that lost their referent when something was edited. Triggered by: finishing a document, "is this right", stating a baseline or floor or threshold, quoting a number, explaining a metric, any claim a reader would take on trust.'
---

# Check Claims

## Core Principle

Wrong survives editing because wrong reads like right.

A false statement can be well written, necessary, non-duplicative, and clearly anchored. Passes for structure, passes for style, still false. Those checks read the document. Correctness lives outside it.

This check asks one question of every claim: **what would make this false, and can I find out?**

---

## The Test

For each factual claim, answer three things:

1. **Where did it come from?** Measured, cited, computed, or assumed.
2. **What would make it false?**
3. **Can I check that in under a minute?** If yes, check it now.

Claims that cannot be sourced get marked as assumptions in the text, or cut. An assumption labelled as one is honest. An assumption phrased as a fact is the failure this skill exists for.

---

## Failure Patterns

### 1. The Neighbour's Fact
A statement true of a related concept, applied to this one. The most dangerous pattern, because the sentence is *almost* right and reads with full confidence.

**Example:** "the base rate is the floor every model must clear." True for accuracy. For AUROC the chance floor is 0.5 regardless of prevalence. One word of the claim was about the wrong metric.

**Test:** name the object the claim is about, then name the nearest concept it could be confused with. Is the claim true of the one you named, or of its neighbour?

Common pairs: accuracy / AUROC · precision / recall · correlation / causation · median / mean · latency / throughput · concurrency / parallelism · authentication / authorization.

### 2. The Unsourced Number
A figure with no traceable origin. Often it was measured once, in a context that has since changed.

**Test:** for every number, say measured, cited, computed, or assumed. If none applies, it was invented — usually from a plausible memory. Either source it or write "approximately" and mean it.

### 3. The Orphaned Figure
A number whose referent was edited away. "Runs in three passes" after one pass was removed. "The third option" after the list became two.

**Symptom:** the number is right about something that is no longer in the document.
**Test:** for each figure, point at the thing that determines it. If you cannot find it in the current text, the figure is stranded.

### 4. Confident About the Untested
A claim about how something behaves, stated flatly, when nobody has run it. Prediction dressed as description.

**Test:** has this been observed, or is it expected? Expected claims take "should", "expect", or "may". Observed ones take the measurement with them.

### 5. The Borrowed Threshold
A cutoff, rule of thumb, or convention imported from another domain without checking it transfers. p < 0.05, 80/20 splits, "under 100ms", the default augmentation recipe.

**Test:** where did this threshold come from, and does the reason it holds there hold here?

### 6. Stale Truth
Was true when written. Versions moved, prices changed, an API was deprecated, a file was renamed, a person changed roles.

**Test:** does this claim have a shelf life? If it names a version, path, price, or person, verify rather than trust.

### 7. The Confident Summary
A summary that sharpens what it summarises. The source said "may", the summary says "does". The source measured one case, the summary states a rule.

**Test:** read the source sentence beside the summary sentence. Did certainty increase between them?

---

## What Is Not a Claim

Do not spend the check on these:

- **Stated intentions.** "I will train a 2D network" is a plan, not a fact.
- **Definitions you are supplying.** "I call this the working resolution."
- **Explicitly labelled assumptions.** Already honest.
- **Opinions offered as opinions.** "This is the stronger option, because X."

The target is anything a reader would take on trust and act on.

---

## Priority

Check in this order, because these differ in what an error costs:

| Priority | Kind of claim |
|---|---|
| 1 | Numbers someone will act on — sizes, costs, deadlines, measurements |
| 2 | Statements about how a method, metric, or tool behaves |
| 3 | Attributions — who said, published, or decided something |
| 4 | Background and framing |

A wrong deadline and a wrong adjective are not the same error.

---

## Self-Check

| Unsourced claims found | Status |
|---|---|
| 0 | Send it |
| 1–2 | Fix and re-read the paragraphs around them |
| 3+ | Stop. The document was written from memory; verify it section by section. |

---

## Worked Example

**Claim:** "I will report each label's base rate as the floor every model must clear."

- **Where from?** Assumed. Carried over from thinking about accuracy.
- **What would make it false?** If the metric's chance level did not depend on prevalence.
- **Check it:** the metric is AUROC. AUROC is the probability a random positive outranks a random negative, so chance is 0.5 whatever the prevalence. **False.**

Pattern 1, the neighbour's fact. It survived a scope review, a vagueness sweep, and a style pass — none of which read for truth. It surfaced only when someone asked what the metric meant.

**Corrected:** "An AUROC of 0.5 is the chance floor every label must clear. I will report each label's base rate alongside its AUROC, since a score built on a handful of positives deserves less confidence than one built on thousands."

The fix kept the base rate but changed its job, from a floor to a measure of how much data the score rests on.

---

## When a Claim Fails

State the correction plainly and move on. Do not delete the surrounding sentence to hide it — the question the claim was answering is usually still worth answering, as above, where the base rate stayed and its role changed.

Then check the neighbours. A claim borrowed from the wrong concept is rarely borrowed alone.
