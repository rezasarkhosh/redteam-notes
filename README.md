# redteam-notes

A running notebook of how I think about AI security — papers I read, ideas I want to chase, and the small "aha" moments I have along the way.

I'm Reza, an MSc student at the University of Padova working on trust-aware federated learning and adversarial ML. My research so far has focused on representation-level robustness — things like client scoring in FL ([COMPSAC 2026 paper](#)) and trust-aware learning for autonomous vehicles in CARLA. I'm aiming for a PhD in adversarial ML / AI security in Europe, so this repo is partly a lab notebook for me and partly a public trail of what I'm actually thinking about.

The code lives elsewhere. Implementations of specific attacks go in [`adv-evasion-from-scratch`](https://github.com/rezasarkhosh/adv-evasion-from-scratch) and similar project repos. This repo is the thinking layer — short markdown notes that accumulate over sprints. Paper summaries, threat models, jailbreak observations I find interesting, attack ideas I don't have time to build yet.

Right now I'm in **Sprint 1: Evasion attacks** — implementing FGSM, PGD, and C&W from scratch. The notes from that sprint live in `sprint-1-evasion/`. New sprints get their own folders as they happen.

## Structure

```
sprint-1-evasion/   notes from the current sprint
papers/             short summaries of papers I read
ideas/              things to explore later
```

Nothing here is polished. It's a working notebook, kept public on purpose.
