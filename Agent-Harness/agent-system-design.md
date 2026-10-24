---
tags: [AI, Agents, System-Design, Architecture, Maddy-Zhang, RAG, Evals]
source: https://youtu.be/UqnLqEcTm5s?si=jH-JIUVkHylxhsLo
channel: Maddy Zhang
speaker: Maddy Zhang
published: September 2026
---

# Agentic AI System Design was HARD until I Learned these 6 Concepts

## Overview: The Core Thesis
In this video, Maddy Zhang (a Senior Software Engineer with experience at Google, Amazon, IBM, and Microsoft) breaks down the fundamental differences between traditional software architecture and **Agentic AI System Design**. 

She emphasizes that an AI agent is not just an API call or a prompt—it is a *probabilistic loop*. Because agents chain multiple reasoning steps, tools, and actions together, standard reliability degrades rapidly with every step. Building a demo is easy, but making an agent reliable enough for production requires strict control over models, tools, memory, orchestration, evaluations, and security.

## The 6 Core Concepts of Agentic Design

### 1. The Agent Loop (Plan -> Act -> Observe)
- An agent operates in a continuous loop: it receives input, plans a step, calls a tool (act), observes the result, and decides if it is finished.
- **The Compounding Failure Problem:** If each step has a 90% success rate, an 8-step workflow drops to a ~43% overall success rate. Reliability erodes with every step you add to the loop. 

### 2. Model Routing
- Do not default to the most expensive, frontier model (e.g., GPT-4o or Claude 3.5 Sonnet) for every step. This burns money and creates high latency.
- **The Strategy:** Use cheap, fast classification models (like Jev or GPT-4o-mini) for easy routing decisions (e.g., "Is this a refund request?"). Reserve heavy reasoning models only for ambiguous, complex tasks where deep judgment changes the outcome.

### 3. Tool Design (APIs for Agents)
- LLMs can only generate text; tools are how they interact with the world. 
- **Tool Sprawl:** If you give an agent 3 tools, it will likely succeed. If you give it 30 tools, its primary failure point becomes simply guessing the wrong tool.
- **Best Practices:** Design tools like strict APIs. Use clear names, exact descriptions, structured inputs, and clean error handling (don't return raw stack traces to the LLM). Strictly separate "read" tools (low risk) from "write" tools (high risk).

### 4. Memory & State Management (Fighting Context Rot)
- **State** is the live execution context (e.g., current order ID, what step the loop is on). 
- **Memory** is long-term data (e.g., past conversations, company policies).
- **Context Rot:** Shoving all state and memory into the prompt for every loop makes the agent "dumber" and much more expensive. 
- **RAG (Retrieval-Augmented Generation):** Treat context as a scarce resource. Store knowledge in a vector database and retrieve only the specific chunks needed for the exact step the agent is currently executing.

### 5. Orchestration (One Agent vs. Swarms)
- Orchestration ties the models, memory, and tools together. 
- For known workflows (like Customer Support), a strict pipeline/state machine is infinitely more reliable than a fully autonomous agent that re-plans from scratch every time.
- **Multi-Agent Swarms:** Use them only when a task truly splits into distinct specialists. Every handoff between agents causes context loss and multiplies the compounding failure math. *A single, well-scoped agent usually beats a swarm.*

### 6. Evals, Observability, and Security
- **Evals (Evaluations):** Because LLMs are non-deterministic, they don't throw standard code errors—they just quietly do the wrong thing. Evals are your automated regression tests. You must build a test suite of happy paths, tool failures, and ambiguous prompts, and run it every time you tweak the system prompt.
- **Observability:** You need trace-level logging. When a customer is angry about a refund, you must be able to trace exactly *which* step in the loop failed (Did it misclassify? Call the wrong tool? Hallucinate?).
- **Security / Blast Radius:** Never put business logic in the prompt. High-risk actions (moving money, deleting data) require hard, deterministic code patterns or human-in-the-loop approvals. Treat all model outputs as potentially hostile (prompt injection risks).

---
*Note: This summary perfectly complements notes on Harness Engineering, AI Agent Tooling, RAG architectures, and Multi-Agent Systems.*
```eof
http://googleusercontent.com/youtube_content/1
