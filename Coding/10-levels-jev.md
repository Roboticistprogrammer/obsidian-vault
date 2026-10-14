---
tags: [AI, Agents, Jev, TypeSafe, IndyDevDan, Harness-Engineering, Cost-Optimization]
source: https://youtu.be/_U-O5lYhJ7Q?si=KBCOLiDqk-k-kwux
channel: IndyDevDan
speaker: Dan (IndyDevDan)
published: September 2026
---

# 10 Levels of Jev For Agentic Engineers

## Overview: The Core Thesis
In this video, IndyDevDan breaks down how to practically integrate **Jev** (the decision-making model by TypeSafe) into agentic workflows. His core thesis is that Jev is not an LLM replacement, but a "third primitive" (a hyper-fast, dirt-cheap classifier) that should be paired *with* LLMs. By offloading routing, reading, and QA tasks to Jev, developers can drastically reduce LLM costs and increase agent safety and speed. 

He structures the guide into 10 progressive levels of complexity for implementing Jev into an agent's harness.

## Level 1-3: Basic Decision Making
These are the foundational uses of Jev, operating as a "smart if-statement" driven by JSON payloads rather than long conversational text.
*   **Level 1: Yes/No Classification:** Used for things like prompt injection detection. Jev can accurately flag malicious prompts (e.g., "ignore all instructions") in under a second for a fraction of a penny.
*   **Level 2: Multiple Choice Options:** Ideal for support triage. You can pass a support ticket to Jev and ask it to classify the issue (e.g., bug, feature request) and its priority (low, normal, high) based on predefined criteria.
*   **Level 3: Composite Scoring:** Jev evaluates multiple weighted variables. For instance, grading the "risk" of a code PR based on whether it touches auth, the size of the change, and test coverage.

## Level 4-6: Agent Routing and Guardrails
This is where Jev starts acting as the "manager" or "security guard" for more expensive LLMs.
*   **Level 4: Confidence Gating:** A wrong answer costs more than asking a human. Jev is used to flag commands (like `git push --force`) as "irreversible" with high confidence, blocking the action before it executes.
*   **Level 5: Intent & Model Routing:** Using Jev as a cheap first-pass router. Based on a user's prompt, Jev decides whether to route the task to a fast model, a browser-specialized agent, or a heavy reasoning model.
*   **Level 6: Tool Hooks (The Bash Gate):** Embedding Jev directly inside the agent harness as a "pre-hook." Every time an LLM tries to execute a bash command or write to a restricted file (like an `.env` or IAM configuration), Jev silently intercepts the command, evaluates its destructive intent, and blocks it if necessary. *This prevents agent hallucinations from destroying codebases.*

## Level 7: Context Management
*   **Level 7: Agent Self-Compaction:** Instead of an LLM blindly accumulating context until it hits limits, Jev runs in the background at specific token milestones (e.g., 6k, 10k, 14k). Jev analyzes the agent's current task versus its previous work and decides if it is time for the agent to trigger a context summarization/compaction protocol.

## Level 8-10: Advanced Agentic Jev (Files & Swarms)
These levels represent a paradigm shift in how agents process large codebases.
*   **Level 8: Cheap File Reads (QA without Reading):** Instead of an LLM reading a 500-line file into its context window just to see if it's relevant, the agent has a tool called `ask_jev_file_bool`. Jev scans the file, answers the agent's question (e.g., "Does this touch auth?"), and the LLM only consumes a few tokens for the answer, not the whole file.
*   **Level 9: Files at Scale:** Scaling Level 8. An agent uses a glob command (e.g., `*.ts`) and asks Jev to run in parallel across dozens of files to find a specific bug or variable. Jev returns a tabular result instantly. The heavy LLM only reads the 1 or 2 files Jev flags.
*   **Level 10: Fully Agentic Jev:** Giving the LLM full autonomy over *how* to use Jev. The agent writes its own JSON payloads to pass to Jev to self-validate its own work (e.g., "Jev, did my test output show that this rounding bug is fixed?"). 

## The Bottom Line
Vibe coding is the floor; agentic engineering is the ceiling. Stop using giant LLMs (like Fable or Opus) for simple categorization, routing, or file-scanning tasks. Embed Jev directly into your custom agent harnesses to achieve massive token savings, near-instant speeds, and deterministic safety guardrails.

---
*Note: This note connects directly with your existing notes on Harness Engineering, AI Agent Tooling, and the TypeSafe Jev architecture.*
```eof
http://googleusercontent.com/youtube_content/1
http://googleusercontent.com/youtube_content/2
