---
title: "Cut The Noise: Building a Code Reviewer You Can Trust"
speaker: Joah Gerstenberg
organization: Block, Inc.
source: https://www.youtube.com/watch?v=f6OVURAlYXI
duration: "~22:00"
type: video-notes
topics:
  - AI Code Review
  - Coding Agents
  - Precision vs Recall
  - Multi-Agent Systems
  - Risk-Based Routing
  - Agent Alignment
status: summarized
---

# Cut The Noise: Building a Code Reviewer You Can Trust

> Summarized from the auto-generated transcript, so names and figures may be slightly off (the speaker's name is transcribed inconsistently; the title page says Joah Gerstenberg).

## Overview

Gerstenberg is a tech lead at Block working on **risk-based agent autonomy**: agents that change production without asking humans for help. His thesis is that models now write code well enough to trust (he dates the shift to around Nov 2025, with GPT-5 and Opus 4.5). **Verification is therefore the new bottleneck in the "software factory"**. Linters, tests and compilers aren't enough, so verification needs intelligence in the same way authorship got it.

Block's own AI reviewer was nearly turned off in 2025 for being noisy and wrong. The talk covers how they rebuilt it around trust.

## Key Concepts

### 1. Code review became a "ritual of trust"

- The average Block reviewer now reviews about **3x as many lines** as a year ago.
- Human attention is spread thin, so reviewers are in effect delegating trust to agents.
- "The code was never the goal." Humans still sign off and are accountable for what agents ship, so the aim is an experience they'd sign their names to.

### 2. Measure offline first, using humans as a measuring stick

- Build a **golden corpus** of hundreds of past human reviews from subject-matter experts in the gnarliest repos.
- Do **not** train the reviewer to imitate past human comments. Those comments come from context agents lack (hidden footguns, unreleased features, last month's sev), so imitation just produces noise.
- Use them to check that the reviewer has enough context to find the same issues.
- Cautionary tale: in April a bug made the reviewer start surfacing what it had learned from past human reviews. This produced "the worst comment I've ever seen" and a **one-week shutdown and rebuild**.

### 3. Precision before recall

| Metric | Meaning | Example |
|---|---|---|
| **Precision** | Of the findings posted, how many are valid? | 10 posted, 7 real, so 70% |
| **Recall** | Of all known bugs, how many were found? | 7 found of 14 known, so 50% |
| **Address rate** (online) | Share of posted findings that operators acted on | Proxy for precision |
| **Post-merge fix rate** (online) | Share of acted-on findings that still needed a fix or revert | Proxy for precision |

- Protect human attention: if the reviewer gets muted, it's useless.
- Others reached the same conclusion: **DoorDash** (minimize false positives), **Figma** (reviewer can't post until provably right at least ~70% of the time), and **OpenAI** (alignment blog: prioritize precision to get a usable reviewer).
- Accept that some low-severity issues will slip through.
- Block's address rate climbed over the year as people saw the findings were accurate.

### 4. Recall is low everywhere, so stack diverse reviewers

- **GitHub's Review Bench** is an open benchmark with labeled PRs. When it launched, Copilot's reviewer led with ~87.8% precision but only **~26% recall**, so it misses ~74% of findings.
- Overlap between different commercial reviewers' findings was only about **1 in 5**, so each finds mostly unique bugs.
- Fix: run **multiple specialist reviewers with different prompts and different models**. This raises recall without hurting precision.

### 5. Architecture: wide, then filter

1. **Specialist reviewers** go wide and report findings with confidence scores.
2. A **coordinator agent** has tool access to company-wide context and tries to *disprove* findings. For example, a "breaking change" claim is dropped if the feature flag is off. It also consolidates duplicates.
3. A **judge agent** reviews the dropped findings and rescues patterns that need human attention.
4. Only findings that survive the whole chain reach humans.

Block open-sourced a version at `block/review` on GitHub, with 6 review agents and layered precision filters. Going from 1 to 6 reviewers raised **high-severity recall from 32% to 50%**.

### 6. Cost and risk-based routing

- High precision and recall need several large models in sequence, which is too expensive to run on everything.
- Inspired by **Meta's** paper on a risk model that predicts how likely a diff is to cause production issues.
- Block built a **risk-based review router**. It looks at privacy and security sensitivity, money movement and SOX compliance, then sends high-risk diffs to expensive review and low-risk diffs to cheap review.
- This lets autonomous agents merge low-risk changes quickly.
- Some systems are still so costly to get wrong that humans want to read the code themselves.

### 7. Alignment and containment

- **Align** agents by giving them business context and tools to look things up: docs, team messages, the experimentation service, email. That way they notice when they're going off track.
- **Contain** their impact so they can't reach aligned goals in unaligned ways. A key tool is a **risk-based adversarial reviewer**.
- Bring teammates into sessions earlier. Block's open-source projects **Buzz** and **Co-Plan** put product, design, engineering and agents in one conversation. "The cheapest review is the one that happens before the work has been done."

## Takeaways

- Measure first, and do the hard work of labeling data.
- Prioritize **precision over recall**. Only surface findings you're confident in.
- Use multiple diverse reviewers, a coordinator that tries to disprove findings, and a judge.
- Route review depth by risk to control cost.
- Build alignment before code is written, and containment after.
- Humans remain accountable. Agents don't take the blame.

## Open Questions / To Try

- Build a small golden corpus of past reviews for my own repos.
- Look at `block/review` for the coordinator and judge pattern.
- Check Review Bench for current precision and recall numbers.

## Related Notes

- [[fighting-code-slop]]
- [[ai_coding_agents_20_agent_workflow_summary]]
- [[Harness-engineering]]
- [[software-factory]]
