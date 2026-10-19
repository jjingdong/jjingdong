# Choosing the coaching model for the Pilates Project

**Question:** which model should write the in-session coaching cues?
**Answer:** `gpt-4o-mini`, chosen over Claude Haiku 4.5 by a blind review. It also turned out to be faster and cheaper.

The Pilates Project guides you through a short Pilates session. During an exercise you can say how it feels ("my neck is getting tight"), and the app answers with two or three coaching cues. The cues are the only part an AI model writes. The session flows are fixed, because AI-generated sequences made the core loop unpredictable.

## The eval set

- **24 cases** across 13 exercises. Each case pairs an exercise with a feeling, plus the answer type the model should give: a correction, a progression, or a safety stop.
- **3 pain cases** never reach a model. A safety check written in code stops the exercise first. A model asked for coaching cues will coach through pain, so safety can't depend on the model behaving.
- Cases also cover retries ("Still feeling it"), vague input ("feels weird") and an off-task prompt-injection attempt.

## How it was judged

1. **Automatic checks** on every reply: valid JSON, two or three cues, a focus line, the right sentiment, banned openers, retries that differ from the first answer, staying on task, and the safety stop.
2. **Blind human review.** I'm a certified Pilates instructor, and I graded each pair of answers with the model names hidden and the order shuffled.

## Results (Oct 2, 2026)

| | gpt-4o-mini | Claude Haiku 4.5 |
|---|---|---|
| Blind review (20 graded cases, 5 ties) | **12 wins** | 3 wins |
| Automatic checks | all pass | all pass |
| Cues over 8 words | **2 of 62** | 44 of 48 |
| Average words per cue | **5.5** | 12.3 |
| Median response time | **1.3 s** | 1.9 s |
| Cost per 1,000 guidance requests | **$0.11** | $1.11 |

Both models were correct on format and safety, so coaching quality decided it. Haiku wrote long, explanatory sentences. gpt-4o-mini wrote short cues that start with a verb ("Lengthen the back of your neck"). A cue has to be usable mid-movement, and that review produced a new product rule: **8 words or fewer per cue**.

## What the eval changed after the choice

Each change was measured on the same 24 cases before it shipped:

| Change | Before | After |
|---|---|---|
| Put the 8-word rule in the prompt | 2 cues over 8 words, average 5.5 words | 0 over, average 4.2 words |
| Instructor "never say" rules (e.g. no "tuck your pelvis"), enforced as an automatic check | 1–2 violations per run | 0 in 5 runs |
| **Bug the eval found:** a retry sometimes repeated its first answer, because the app never sent the first cues back to the model | retries different: 1 of 2 | 2 of 2, in 3 of 3 runs |

## Caveats

- 20 graded cases from one reviewer is a small sample. It's enough to choose a model, not to claim a precise win rate.
- The prompt was first written for gpt-4o-mini. A prompt tuned for Claude could narrow the gap, so the eval gets re-run before any future model switch.

[← Back to profile](../README.md)
