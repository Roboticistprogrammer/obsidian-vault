---
tags: [AI, Agents, Claude-5.5, Agentic-OS, ARMS-Framework, System-Design, Jev]
source: https://www.youtube.com/watch?v=s_1GPN-NXA0
channel: Jay E | RoboNuggets
speaker: Jay E
published: October 2026
---

# A NEW Agentic OS Standard for Claude 5.5 is here

## Overview: The Core Thesis
In this video, Jay E breaks down his personal "Agentic Operating System," optimized for the latest Claude 5.5 model family, the Jev decision model, and Claude Mods. He argues that building a customized, token-efficient workspace is essential to leverage AI without burning through usage limits. 

He structures his system around the **ARMS Framework** (Apps, Routines, Memory, Skills), providing three distinct "levels" of mastery for each pillar. The overarching theme is shifting workflows *out* of standard chat interfaces and into highly structured, self-improving micro-apps and background processes.

## The ARMS Framework

### 1. Skills (Saved Instructions)
*   **Level 1: Skill Types & Wildcards:** Differentiate your skills. *Database skills* hold knowledge (e.g., tone of voice guidelines, thumbnail references). *Artifact skills* standardize outputs using HTML/CSS for brand consistency. *Self-triggered skills* run maintenance tasks outside of chat. *Pro Tip:* Add "Wildcards" to database skills to prevent the LLM from "overfitting" and losing its creativity.
*   **Level 2: Model Routing (via Jev):** Don't use expensive frontier models for spelling checks. Jay uses **Jev** to intercept skill calls and route them dynamically. Simple skills get sent to Claude 5.5 Haiku, saving massive amounts of tokens and API credits (e.g., running 4,000 routing decisions for just $0.18).
*   **Level 3: Self-Improving Skills:** Build feedback loops. Whenever Jay leaves a comment on an agent's work, the agent automatically appends instructions to update its own core skill markdown file, creating a continuous reinforcement learning loop.

### 2. Routines (Scheduled Tasks)
*   **Level 1: Active vs. Passive Routines:** *Active* routines give you a daily output (e.g., an inbox digest). *Passive* routines run silently in the background and only ping you if a condition is met (e.g., a "Brand Watch" script checking for domain squatters). Rely heavily on passive routines to avoid notification fatigue.
*   **Level 2: Capped Routines:** Monitor the token drain of scheduled tasks. Assign hard API budget caps (e.g., "$4/week") to specific routines so background tasks don't accidentally burn through your entire usage allocation.
*   **Level 3: Routines that Suggest Routines:** Have an end-of-week agent process that reviews your manual chat sessions, identifies repetitive tasks, and suggests new routines to automate them.

### 3. Memory (Context & Retrieval)
*   **Level 1: Long-Term Memory (The Plain Text Map):** Jay manages a 400,000-file "Second Brain" *without* heavy Vector/RAG databases. Instead, he uses a 3-layer map: 
    1. `memory.md` (a high-level map auto-loaded by Claude).
    2. `claude.md` (a master router that points to department-specific files like `business.md`).
    3. *Database Skills* to pinpoint specific data. Plain text retrieval is currently outperforming complex RAG setups for 5.5 models.
*   **Level 2: Short-Term Memory (Cache Management):** Claude 5.5 has a 1-million token context window, but letting a chat sit idle for 1 hour drops the "Prompt Cache," making the next prompt extremely expensive. Jay uses a background routine to "self-ping" his most important heavy sessions every 50 minutes, keeping the cache warm and saving hundreds of dollars in reload costs.
*   **Level 3: Self-Evolving Memory:** Use a `/note` skill to park random ideas/fixes during the day without derailing the current chat context, then process those updates in batches later.

### 4. Apps (External Integrations & UIs)
*   **Level 1: Connector Hierarchy:** When connecting agents to external apps, prioritize connection types to save tokens. The hierarchy is: Official Connectors -> CLI (Command Line Interface - lightest) -> API -> MCP (Model Context Protocol - heaviest, as it loads the full tool spec into context on every chat).
*   **Level 2: Mods & Extensions:** Use "Claude Mods" to alter the desktop app UI (e.g., adding a persistent "Token Usage" or "Prompt Cache" status bar). Leverage Claude to build custom Google Chrome extensions to grab code or images without leaving the browser.
*   **Level 3: Micro-Applications:** Stop using the chat window for everything. Have Claude build lightweight, local, browser-based micro-apps (using a unified UI component library) to manage complex tasks like reviewing video edits or approving Instagram carousels.

---
*Note: This note ties directly into the concepts of Proactive Agents (IndyDevDan) and Harness Engineering (Mike Chambers), emphasizing token economics, cache warming, and moving beyond conversational AI into deterministic operating systems.*
```eof
http://googleusercontent.com/youtube_content/1
