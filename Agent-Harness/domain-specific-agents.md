---
title: "Why Domain-specific Agents Are the Future"
speaker: Justin Schroeder
organization: Standard Agents
source: https://www.youtube.com/watch?v=wIPr9GmLRaQ
duration: "~25:00"
type: video-notes
topics:
  - Domain-Specific Agents
  - Agent Harness
  - Agent Standards
  - Model Selection
  - Agent Evals
status: summarized
---

# Why Domain-specific Agents Are the Future

> Summarized from the auto-generated transcript, so product and model names are sometimes garbled (e.g. "Fable", "Astra", "Grok Bot"). Treat specific names and numbers as approximate.

## Core Thesis

We are in the **line-shaft era of agents**. 19th-century factories ran one giant steam engine with belts and shafts to every workstation. Today we put one giant model in the middle and tie every task back to it with skills and MCP. The speaker calls this the "Claude God" pattern.

The future is **many tiny agents, each built around one or two tasks**, each with its own "power supply". Examples are a Gmail agent, a Figma agent and a Sheets agent, tied together by an orchestrator, possibly via MCP.

## Definition of an agent

> **Deterministic software that uses the non-deterministic output of a model in pursuit of an objective.**

- This is the same thing as a "harness". He thinks the agent/harness distinction is no longer useful.
- The loop (LLM, tool calls, feed results back) is easy. The hard parts are orchestration, async handling, provider abstraction and durable execution.
- Despite "don't build agents, build MCP servers" advice, everyone ends up building agents anyway, even behind MCP calls.

## Why domain-specific: integration, trust, efficacy

### 1. Integration
- Businesses want automation woven into existing **processes** (his example: fleet maintenance at an HVAC shop), not one omnipotent AI owning the whole business.
- Mainstream businesses aren't interested in handing everything to one model, even one that works.

### 2. Trust
- An agent is a model plus system prompt, functions, sub-prompts, sub-agents, hooks, **agent rules** (e.g. step limits), and optionally a filesystem and code execution.
- A narrow agent **literally lacks the tools** to wander. Ask the Gmail agent to check Twitter and it says no. A skill-driven general agent might "helpfully" shut down prod.
- Bounded step counts matter: the HVAC shop doesn't want an agent that can run forever.
- Old engineering principles apply: **least privilege, separation of concerns, composition over inheritance, law of Demeter.**

### 3. Efficacy
- **Public benchmarks are untrustworthy.** Quote he cites: the biggest victim of benchmarks is the people too busy to try the models themselves.
- Narrow agents are **much easier to eval** on the exact business process (e.g. "check Gmail correctly every time").
- Models are **jagged**: bad at most things but great at one. His Model Zen Garden demo shows a weak general model producing a better 3D world than a frontier model, for about **$0.60**.
- Use the right model per agent for **big token and cost savings**. He claims DeepSeek V4 Flash is **~137x cheaper per task** than a frontier model, and fine for small tasks.
- Cost: you can't swap models freely and expect the same behavior, so the agent is built *for* a specific model.

## The electric motor analogy

- The electric motor alone did **not** transform factories. Swapping it in for the steam engine changed little for ~30 years. This is like upgrading the model under the same skills.
- What unlocked it was **standards** (voltage, frequency, etc.). A 1910 machine and a 2021 machine can take the same power.
- So the unlock for agents is **standards**, and none exist yet.

## What agents need (his wish list)

1. **Domain-specific** and small.
2. **Full, editable harness** that behaves identically when installed on someone else's machine.
3. **Portable**: installable like `npm install` / `pip install`.
4. **Vendor-agnostic**, but pinned to the same model, provider and quant so efficacy is guaranteed.
5. **Composable**: a standard way for agents to talk to each other.

Standard Agents (his company) plans an **open-source spec** and a **runtime** for installing agents, with competing runtimes, as with Node and Python.

## Q&A highlights

- **Encode in the agent or fine-tune the model?** Mostly encode in the agent, but the model is part of the design. Switching models in a tool and "firing stuff at it" is an anti-pattern long-term because there's no verifiability.
- **Governance as models change?** Use platforms where IT sees all small agents and owns **private evals**. Don't upgrade just because a new frontier model exists. If it does the process right, **leave it until the model is deprecated.**

## Takeaways

- Break work into **narrow, process-shaped agents** instead of one general agent with many skills.
- Narrow scope gives better trust (least privilege), cheaper tokens and tractable evals.
- Run **your own evals**, not public benchmarks, and pick models per task.
- Don't upgrade models just because a new one exists.
- The missing piece is **standards** for packaging, running and composing agents.

## Related Notes

- [[Harness-engineering]]
- [[Agent-OS]]
- [[agent-system-design]]
- [[software-factory]]
- [[managed-agents]]
- [[cut-the-noise-trusted-code-reviewer]]
