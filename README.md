# Kenny Perez, MD — AI Projects

I'm a double board-certified psychiatrist (adult; child & adolescent) and the Chief Medical Officer of Elevate Psychiatry, a multi-site outpatient practice in South Florida. I build the AI systems my practice runs on.

This repository is a public showcase. **The source code is private** because it runs against live clinical and HR systems. Each page below describes what the system does, how it is built, how it is evaluated, and what I learned. I'm happy to walk through the code live on request.

## Featured projects

| # | Project | What it is | Why it matters for AI safety |
|---|---|---|---|
| 1 | [Lola — local AI phone receptionist](projects/01-lola-voice-receptionist.md) | Bilingual voice agent for a psychiatric practice, with every model running on one Mac Studio | Deterministic safety layers around the LLM, crisis routing that never reaches the model, and a held-out evaluation built from real calls |
| 2 | [Lola HR — autonomous recruiting agent](projects/02-lola-hr-agent.md) | Claude-backed agent that screens, messages, schedules, calls and scores candidates across two companies | A system that worked as designed and still caused harm to the people around it |
| 3 | [PharmaBot — calibrated FDA forecasting](projects/03-pharmabot-calibration.md) | Multi-agent pipeline that estimates the probability of FDA approval for psychiatric drugs | Honest calibration: the data agents alone show weak discrimination, and clinician judgment was the only feature that separated cases |

## How I work

- **The model talks; the code decides.** Anything that affects safety or correctness is enforced in deterministic code, not left to a prompt.
- **Fail toward a human.** When a system is unsure, it hands off to a person rather than guessing.
- **Measured, not assumed.** Changes are judged on held-out data, and negative results are written down.

These systems were built with Claude Code as my coding collaborator. The product and clinical requirements, safety policies, evaluation design and deployment decisions are mine.

## Other work

- An automated equity research and trading system with an event-driven backtester. Its most useful moment was catching survivorship bias in its own backtests, withdrawing those results and rebuilding on a point-in-time data store.
- A Microsoft Teams integration and agent plugin that gives AI agents controlled access to messaging, meetings and transcripts.
- An internal CRM for the practice and an SEO agent for practice content.

## Contact

[LinkedIn](https://www.linkedin.com/in/perezpsychiatry/)
