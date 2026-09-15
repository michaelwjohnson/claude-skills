---
name: occams-razor
description: 'Use when designing, scoping, or reviewing anything with parts — a system design, experiment, spec, plan, document, or API — and especially when adding a component, comparison, model, metric, section, or option. Catches elements added for symmetry, completeness, or elegance rather than because a question requires them. Triggered by: "should I also add", a design that has grown since it was proposed, plans with multiple arms or comparisons, scope review, anything that feels thorough.'
---

# Occam's Razor

## Core Principle

> "The supreme goal of all theory is to make the irreducible basic elements as simple and as few as possible **without having to surrender the adequate representation of a single datum of experience**." — Einstein

**Both halves bind.** The first half cuts. The second half stops the cutting.

This is not minimalism. A required element is never excess. Dropping something that carries a claim is the same failure as adding something that doesn't — it just fails in the other direction.

---

## The Test

Before adding any element, name the question it answers.

Then check:

1. **Is that question already answered** by something already present?
2. **Is that question the one being asked?**

If either check fails, the element does not go in.

Apply the same test in reverse when reviewing: for each element present, name its question. Elements with no question are the ones to cut.

---

## Failure Patterns

### 1. Filling In
Something looks unfinished, so it gets finished. A 2×2 with an empty cell. A list of two that "should" be three. Every metric when one decides, every configuration when three are informative, every case when the question needs one.

**Symptom:** the reason for the element is the shape of the design rather than a question. Completeness is not evidence of need.
**Tests:** if the gap were never noticed, would anyone ask for what fills it? And of the cases covered, which would change a decision?

### 2. The Second Question
A design starts with one question and acquires another through a comparison nobody requested. Now two studies share one deliverable and neither gets full attention.

**Symptom:** the stated question and the actual experiments have drifted apart.
**Fix:** split into two deliverables, or cut one question.

### 3. Duplication Across Sections
An extension answers the question the core already answered. A summary restates the analysis. A test covers what another test covers.

**Symptom:** removing one section loses nothing.
**Fix:** one of them is in the wrong place. Decide which, move or cut.

### 4. The Unnamed Cost
An addition is proposed with its benefit stated and its cost silent. Runs, days, pages, latency, maintenance, reader attention.

**Test:** an addition whose cost is not named is being sold, not proposed. State both or don't propose it.

### 5. Compensating Additions
Something doesn't work, so a part is added to handle it. Then a part to handle that part.

**Test:** try removing the original part first. A layer added to fix a layer is usually two layers too many.

### 6. Speculative Parts
"We might want this later." An option nobody asked for. A hook for a use case that doesn't exist.

**Test:** if it isn't needed now, it is a guess about the future dressed as a requirement.

### 7. Relocation Without Rechecking
An element is cut from one section and moved to another rather than removed. It left the first section for a good reason. Nobody asked whether it fits the second.

**Symptom:** a part that no section's stated question actually needs, sitting where it was last put down.
**Test:** moving is not cutting. Apply the test again at the destination, against *that* section's question, not against the reason it left.

### 8. Orphans of a Cut
A part is removed and the sentences that served it stay: a justification for something no longer present, a control describing a comparison that no longer exists, a fallback naming a model that moved, a summary counting elements that changed.

**Symptom:** references that do not name the removed part, so a search for it comes back clean.
**Test:** after cutting, ask what was *supporting* the removed element and what was *adjacent* to it. Read the surrounding sections rather than grepping for the name.

---

## What Is Not Excess

The second half of the quote. Do not cut:

- **Stated requirements.** A rubric item, a spec requirement, a user's explicit ask.
- **Controls that isolate the variable actually under study.** The comparison that answers the question is the point, not decoration.
- **Baselines and floors.** Without them a result has no scale.
- **Honest limitations.** Removing them makes the work look stronger and be weaker.
- **Error handling on paths that can fail.**

Cutting these is not simplicity. It is under-delivery wearing the razor's clothes.

---

## What This Check Does Not Do

Passing means an element earns its place. It says nothing about whether the element is **correct**.

A false claim can carry a real question, duplicate nothing, and survive every pattern above. Fluent and wrong looks exactly like fluent and right from inside the document, because this check reads structure and correctness lives outside it.

Run a claim check separately. Do not fold one into the other: the moment this skill also asks "is it true", it is answering two questions, which is pattern 2.

---

## Self-Check

For each element in the design, write the question it answers. Then count:

| Elements with no question | Status |
|---|---|
| 0 | Clean |
| 1 | Cut it |
| 2+ | The design has drifted. Re-derive it from the stated question rather than patching. |

Also count distinct research questions. **More than one per deliverable is the most common structural failure**, and the hardest to see from inside.

---

## Before / After

**Before** — an experiment that grew:

> Four arms: current checkout, current checkout with a progress bar, one-page checkout, one-page checkout with a progress bar. Run on identical traffic. The follow-up study compares progress-bar styles.

The stated question is whether one-page checkout converts better. Applying the test:

- Arm 1 vs 3 → *does one-page convert better?* This is the stated question. Keep both.
- Arm 1 vs 2 → *does a progress bar help?* Not the stated question. **Second question.**
- Arm 4 → completes the 2×2. **Filling in.**
- Follow-up → *which progress bar is best?* **Duplicates** the ground arm 2 introduced.

**After:**

> Two arms: current checkout and one-page checkout, answering whether one-page converts better. The follow-up study compares progress-bar styles.

One question per deliverable, and the design is smaller rather than rearranged.

**What the first pass got wrong.** It moved the progress-bar arms into the follow-up instead of cutting them, on the reasoning that "the follow-up is about progress bars." But the follow-up's question was narrower — which *style* wins once a bar is present. Whether to have one at all is a different question, and no section had asked it. The arms were checked against the reason they left, never against the question at their destination. That is pattern 7.

Cutting them then left three orphans elsewhere: a hypothesis still naming two effects, a sample-size calculation still powered for four arms, and a rollout plan referring to an arm that no longer existed. None of them named the removed arms, so searching for them came back clean. That is pattern 8.

---

## When Cutting

Say what was cut and why, so the decision stays visible and reversible. A silent cut is indistinguishable from an oversight, and the next reader will add it back.

Then do two more things:

1. **If the element moved rather than went, test it again where it landed** — against that section's question, not against the reason it left.
2. **Sweep for orphans.** Ask what supported the removed element and what sat beside it. Justifications, controls, counts, and fallbacks referring to it usually do so without naming it, so searching for the name will not find them. Read the neighbouring sections.
