# Hi, I'm Jing

**Engineer-turned product manager in New York. I build AI products end to end, from the problem definition to the working build, and I measure the AI with evals before I trust it.**

[![Website](https://img.shields.io/badge/jjingdong.com-Portfolio-black?style=flat-square)](https://jjingdong.com)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-0A66C2?style=flat-square&logo=linkedin)](https://linkedin.com/in/jjingdong)

---

### Products

| Product | What it is | Links |
|---|---|---|
| **Curve Routine** | Helps people stick to the home exercise routine their physical therapist gave them. Type the exercises in your own words, and it turns them into a guided session you can follow, log, and bring back to your next appointment. Built for Lovable SheBuilds Season 04 (Oct 2026). | [Live demo](https://curve-routine.lovable.app) · [Eval](evals/curve-routine-reader.md) |
| **Pilates Voice** | Short guided Pilates sessions with an AI coach. Say how an exercise feels, and it answers with two or three short cues. The session flows are fixed, and a safety stop in code catches pain before any model is called. | [Live demo](https://pilates-voice.vercel.app) · [Eval](evals/pilates-coach-model-choice.md) |
| **[ai-pm-job-radar](https://github.com/jjingdong/ai-pm-job-radar)** | Tests whether Jev, a decision model, can sort PM job listings better than a keyword filter. Out of 7,301 listings, it found 137 AI-related PM roles for under five cents. | [Repo](https://github.com/jjingdong/ai-pm-job-radar) · [Eval](https://github.com/jjingdong/ai-pm-job-radar/blob/main/eval/results.md) |

### Eval case studies

How I decide whether an AI feature is good enough to ship.

| | |
|---|---|
| **[Zero invented numbers](evals/curve-routine-reader.md)** | Curve Routine: a 13-note eval where hallucinated exercises and invented values are hard rules, not averages |
| **[The blind review that picked the model](evals/pilates-coach-model-choice.md)** | Pilates Voice: gpt-4o-mini vs. Claude Haiku 4.5, judged blind by a certified instructor, plus a retry bug the eval caught |
| **[Model vs. keyword filter](https://github.com/jjingdong/ai-pm-job-radar/blob/main/eval/results.md)** | ai-pm-job-radar: 40 hand-labeled postings. On "is this AI product work?", the model scored 15/15 and the title regex 5/15 |

### Build case studies

| | |
|---|---|
| **[A feature flag for the AI model](case-studies/pilates-voice-model-flag.md)** | Pilates Voice: roll a new model out to a share of users, keep each user on one model, and roll back automatically when it starts failing |

### Background

Fifteen years in tech: seven as a backend software engineer, six teaching and coaching engineers, and the last year and a half in product.

- **Independent AI product builder** (2025–present): designing and building Curve Routine and Pilates Voice solo, covering product architecture, AI interaction design, evals, and UX
- **Product Manager, ApplyPass** (2023–2024): owned roadmap and release planning for a platform that helps software engineers find and automatically apply to jobs
- **Tech Coach Manager, Outco** (2021–2023): taught 500+ hours of data structures and system design, grew the coaching team from 20+ to 40+ supporting 1,000+ learners, and contributed to 300+ job placements
- **Platform Engineer, Refinery29** (2015–2016): backend for an article feed serving 110M page views a week and a video platform that grew views 300%

### Toolkit

`Python` `SQL` `Git` `OpenAI API` `Claude API` `Claude Code` `Cursor` `Lovable` `Supabase` `Google Cloud`
