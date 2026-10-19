# Evaluating Curve Routine's routine reader

**Question:** can a model turn messy, typed notes about a physical therapy routine into a correct exercise list, without inventing anything?
**Answer so far:** yes, on 13 test notes, with zero hallucinated exercises and zero invented numbers. There are caveats below.

Curve Routine helps people stick to the home exercise routine their physical therapist gave them. You type the exercises in your own words, the reader turns them into a structured list, and you check that list before it becomes your routine. The reader is the one AI step in the app.

## Why the rubric starts with safety

An app that "helpfully" adds an exercise your PT didn't give you, or fills in "3 sets" because that's typical, breaks the product's promise. So two metrics are hard rules, and the rest are targets:

| Metric | Definition | Bar |
|---|---|---|
| **Hallucinated exercises** | exercises in the output that aren't in the text | **0** |
| **Invented values** | numbers filled in that the text doesn't state | **0** |
| Exercise recall | ground-truth exercises found | ≥ 95% |
| Field accuracy | sets, reps, hold and side correct | ≥ 90% |
| Low-confidence flags | ambiguous fields marked for the user to check | ≥ 80% |
| No-exercise text | returns an empty list instead of guessing | 100% |

## The eval set

13 notes, each with a hand-written ground truth. Four are based on real home-exercise routines (personal details removed), and the rest are written to be hard on purpose:

- terse lines and shorthand ("bridge 3x10 hold 5", "clamshell 2x15 ea side")
- no numbers at all, which tests whether the model invents values
- run-on sentences and typos ("wall angles")
- a pasted clinic note with clinic-only treatment mixed in, which must not be extracted
- traps that look like exercises: "ice", a question about running
- different amounts per side ("15 L / 10 R") and unsure numbers ("10, maybe 15")
- a 22-exercise routine
- the app's own suggested inputs, so the path judges would click is tested

A field the note doesn't state is `null`, even when the answer seems obvious. A scorer (`score.py`) checks every field, and it was self-tested first: the ground truth scores 100%, and planted errors get caught.

## Results

| Date | Run | Recall | Hallucinated | Field accuracy | Invented | Must-flag |
|---|---|---|---|---|---|---|
| Oct 11 | Prompt v1, helper agents (upper bound only) | 70/70 | 0 | 350/350 | 0 | 5/5 |
| Oct 12 | Prompt v1, Claude API (`claude-opus-5-5`, low effort) | 77/77 | 0 | 385/385 | 0 | 5/5 |
| Oct 16 | New prompt in the live app, spot check on 5 notes | correct on all 5 | 0 | — | 0 | flagged as expected |

The Oct 16 check ran through the deployed backend, as an anonymous user, with made-up notes only. For example, "bridges 10 reps, maybe 15?" came back as 10 reps, flagged for review. "20 or 30 sec" came back as no hold time, flagged. Uneven sides came back as left 15 / right 10.

## Caveats

- **The same model family wrote the prompt, the ground truth and the answers.** That makes these numbers an upper bound, not proof.
- 13 notes is small. The target is 20–30, plus a stability re-run and a smaller-model comparison on cost and speed.
- The live metric that matters most, how often users edit fields after extraction, starts with the beta.

[← Back to profile](../README.md)
