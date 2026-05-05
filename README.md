# redteam-notes

> A living, public notebook for AI security and adversarial ML work — short notes, paper takeaways, attack ideas, and aha moments captured *as I learn*, not after.

[![Status](https://img.shields.io/badge/status-active-brightgreen.svg)](#)
[![Sprint](https://img.shields.io/badge/sprint-1%20%28evasion%29-red.svg)](#currently-working-on)
[![License: CC0-1.0](https://img.shields.io/badge/license-CC0--1.0-lightgrey.svg)](https://creativecommons.org/publicdomain/zero/1.0/)

## What this repo is

This is my **public lab notebook** for AI security and adversarial machine learning. Every two weeks I run a focused implementation sprint — pick one attack or defense family, reimplement it from scratch in PyTorch, ship a small public artifact, move on. Full curriculum lives in [my AI Security Skill Sprints plan](#).

Along the way, this repo is where I capture:

- aha moments that probably won't fit into a paper
- short paper takeaways with my own framing
- threat-model sketches and attack ideas I haven't yet had time to try
- the hard bugs I hit and how I diagnosed them
- one-liners I want to remember six months from now

It's **deliberately messy.** The whole point is low-friction capture — if writing a note takes more than 10 minutes, the system has failed.

## What this repo is *not*

- **Not a project repo with shippable code.** That's [`adv-evasion-from-scratch`](https://github.com/<your-username>/adv-evasion-from-scratch) — go there for FGSM, PGD, and C&W from scratch.
- **Not a polished blog.** End-of-sprint blog posts get published on Medium / personal site after editing.
- **Not academic writing.** Papers and formal drafts live elsewhere.

If you want to know what I've *built*, look at my pinned project repos. If you want to know how I *think*, this is the right place.

## Currently working on

🟢 **Sprint 1 — Evasion attacks** (FGSM → PGD → C&W) · Day 5 of 14
📁 Notes folder: [`sprint-1-evasion/`](sprint-1-evasion/)
🔗 Project repo: [`adv-evasion-from-scratch`](https://github.com/<your-username>/adv-evasion-from-scratch)

## Repo structure

```
redteam-notes/
├── README.md                              ← you are here
├── sprint-1-evasion/
│   ├── day-1-fgsm-paper-notes.md
│   ├── day-3-fgsm-aha.md
│   └── day-5-pgd-notes.md
├── sprint-2-poisoning/                    ← future
├── sprint-3-mia/                          ← future
├── papers/                                ← standalone paper summaries (not tied to a sprint)
├── ideas/                                 ← things to explore later
└── threat-models/                         ← system threat-model sketches
```

The sprint folders are the spine of the repo. Each gets its own folder when the sprint starts. Longer-running directories (`papers/`, `ideas/`, `threat-models/`) hold material that doesn't belong to a single sprint and grows over time.

## How I use this repo

- **One commit per note.** No batching, no waiting for "when it's ready."
- **Filenames carry the date when it matters.** Sprint notes use `day-N-topic.md` so the timeline is visible from the file tree alone.
- **Notes are short.** Bullets, not essays. If a note grows into an essay, it gets promoted to a blog post — and a link back here.
- **Past me writes for future me.** If a note is unclear in two weeks, I rewrite it; I don't pretend it was clear all along.

## Sprint plan (next ~3 months)

| # | Topic | Status |
|---|---|---|
| 1 | Evasion attacks (FGSM → PGD → C&W) | 🟢 in progress |
| 2 | Data poisoning variants (BadNets, Poison Frogs) | ⬜ planned |
| 3 | Membership inference & privacy attacks | ⬜ planned |
| 4 | Differential privacy (DP-SGD from scratch) | ⬜ planned |
| 5 | FL attacks & robust aggregation | ⬜ planned |
| 6 | LLM red-teaming (jailbreaks + prompt injection) | ⬜ planned |

Sprints 7+ get planned at the end of Sprint 5 — when there's actual data on what excites me.

## About me

Reza Sarkhosh — MSc candidate in *ICT for Internet and Multimedia: Machine Learning for Healthcare* at the University of Padova, currently visiting TalTech (Estonia) for thesis research on federated learning security.

Selected work:

- *Trust-Aware Client Scoring in Federated Learning via Contrastive Attention and Representation Clustering* — IEEE COMPSAC 2026 (full paper, 28.5% acceptance).
- Backdoor data-poisoning study on CARLA-based traffic-sign detection.

Long-term goal: AI Security Engineer at a FAANG-tier lab. This repo is part of how I'm getting there.

[GitHub](https://github.com/<your-username>) · [LinkedIn](https://linkedin.com/in/<your-handle>) · [Email](mailto:<your-email>)

## License

[CC0 1.0](https://creativecommons.org/publicdomain/zero/1.0/) — public domain dedication. Take any note that helps. Attribution is welcome but not required.
