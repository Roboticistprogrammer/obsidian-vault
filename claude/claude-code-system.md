# Anthropic's New System for Using Claude Code — Summary

> **Note:** This summary is based on public reporting about Anthropic's Claude Code review workflow (Daily.dev, Oct 29, 2026), not on the video's transcript, which was not accessible. The video title and date align with this topic.

## Overview

Anthropic's Claude Code team introduced a structured review workflow for AI-generated code changes. According to reporting, the system **more than doubled the number of changes shipped** while **cutting reverted changes by over 40%**. Reviewed changes across Anthropic rose from roughly **2,500 to over 30,000 in four months**.

## Core Components

### 1. Pre-build "Evals"
- Fixed tests that Claude must pass **before** it can mark any work as done.
- Prevents the agent from prematurely declaring success.

### 2. Separate Reviewer Agent
- A distinct reviewer agent with a **fresh context window** examines every change before a human sees it.
- Rationale: the agent that built a change tends to agree with its own decisions, so self-review is unreliable.

### 3. Risk Classifier
- Determines how strict the review needs to be for each feature.
- Not every change goes through the same level of scrutiny.

### 4. Cap on Review Loops
- The reviewer is limited to **three rounds**.
- After that, it stops listing new problems and tells the builder to rethink its approach.
- This rule was added after one change looped **more than 50 times**, with **59% of flagged issues being repeats** from the first round.

## Why It Matters

- Addresses **self-assessment bias** in AI coding agents.
- Separating builder from reviewer and enforcing objective completion criteria catches issues the original agent would have missed or approved itself.
- Results in more changes shipped with fewer reverts.

## Key Takeaways

| Metric | Before | After |
|---|---|---|
| Changes reviewed | ~2,500 | 30,000+ |
| Reverted changes | Baseline | -40%+ |
| Review loop cap | None | 3 rounds |

---

*If you can share the video's transcript, I can revise this summary to reflect the video's specific content and framing.*
