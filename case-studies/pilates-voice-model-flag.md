# A feature flag for Pilates Voice's AI model

**Question:** once an eval picks a new model, how do I switch to it without betting every user on it at once?
**Answer:** a feature flag on the server. It sends a set share of users to the new model, keeps each user on the same model, and switches everyone back on its own if the new model starts failing.

Pilates Voice guides you through a short Pilates session. During an exercise you can say how it feels, and an AI model answers with two or three coaching cues. The model is the one part of the app that can change underneath users. I chose the current one, gpt-4o-mini, with a [blind review](../evals/pilates-coach-model-choice.md) on 24 test cases. An offline eval is a good filter, but it isn't real traffic.

## What the flag does

| | How it works |
|---|---|
| **Gradual rollout** | Two settings: which model to try, and what percentage of users get it. Everyone else stays on the current model. |
| **Same model every time** | Each browser gets an anonymous random id. The server turns it into a number from 0 to 99, so a user's bucket never changes. Set the rollout to 20% and buckets 0–19 get the new model. |
| **Kill switch** | Set the percentage to 0 and everyone is back on the current model. There's no code change, only a settings change. |
| **Automatic rollback** | If more than 30% of the new model's last 20 requests fail (with at least 5 seen), everyone goes back to the current model for 10 minutes. Then the new model gets another try. |
| **Measurement** | Every reply says which model served it, so the two groups can be compared. |

The safety stop doesn't change. If someone reports pain, the app stops the exercise in code before either model is called.

## How I tested it

I didn't want to spend real model calls, or wait for real failures, to know whether the rollback works. So I tested the whole request path against a fake model service:

| Test | Result |
|---|---|
| 20% rollout across 10,000 ids | 19.97% got the new model |
| Same id, called again | same bucket every time |
| Rollout at 0%, or no model set | nobody gets the new model |
| New model fails every request | 5 failed replies, then automatic rollback; the next replies came from the current model and worked |
| "Sharp pain in my back" | safety stop, no model called |

## What I'd tell an engineer reviewing it

- **The rollback isn't global.** Vercel runs several copies of the server, and they don't share memory. Each copy counts its own failures and rolls back on its own. To make it global, I'd keep the counts in a shared store, or have a monitoring alert set the rollout to 0. That second option is the case for flags and monitoring being one product.
- **Errors aren't the only way a model fails.** A model can answer every request and still write cues that are too long to use mid-exercise. The eval already checks for that (8 words or fewer per cue). The next step is to feed those quality checks into the rollback, not just errors.
- **There's nothing to compare yet.** Pilates Voice has no users, so the two groups have no results. The flag is tested, not measured.

## Why I built it

Picking a model is half the decision. The other half is how you roll it out: who gets it first, how you know it's working, and how fast you can undo it. This is my smallest working version of that.

[← Back to profile](../README.md)
