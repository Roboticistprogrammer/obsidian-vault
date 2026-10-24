---
tags: [AI, Agents, Jev, Pi-Durable, Proactive-Agents, News-Curation, IndyDevDan]
source: https://www.youtube.com/watch?v=OLqP8jxJ0Wc
channel: IndyDevDan
speaker: Dan (IndyDevDan)
published: October 2026
---

# My Jev Pi Durable Agent CURATES News: HYPE, FACT, or SIGNAL

## Overview: The Core Thesis
In this video, IndyDevDan introduces the concept of **Proactive Durable Agents**. The core thesis is that the mark of an elite agentic engineer is transitioning from being "Human-in-the-loop" (chatting directly with an LLM) to "Human-near-the-loop," and eventually "Human-out-of-the-loop." 

To demonstrate this, Dan builds a fully autonomous, durable News Curation Agent using Pi Durable and Jev. Instead of waiting for the user to prompt it, a proactive agent runs continuously on a cron schedule, actively gathering, deduplicating, and ranking information in the background. The human only steps in occasionally to extract value and provide feedback. 

## Key Concepts & Workflows

### 1. The In -> Near -> Out of the Loop Framework
- **In the Loop:** You sit at the terminal manually prompting an agent back and forth to get a task done.
- **Near the Loop (Proactive):** The agent runs autonomously in the background on a schedule. It works for you, then pauses, waiting for you to step in, review its work, and provide feedback. 
- **Out of the Loop:** The ultimate goal. Fully autonomous systems compounding value continuously without any human intervention.

### 2. Information Triage: Hype, Fact, and Signal
Dan configures his news agent to classify incoming tech news into three strict buckets:
- **Hype:** High attention, low actual value (e.g., "AI coding has increased 10x!"). Driven by unfounded excitement.
- **Fact:** Objectively true, but not immediately actionable for an engineer (e.g., a massive math proof dataset is released by a research lab).
- **Signal:** Objectively true *and* immediately actionable (e.g., the API weights for Gemini 4 Argon are officially dropped).

### 3. Architecture of a Proactive Durable Agent
The system is built in distinct layers to ensure it acts as a background product rather than a simple chat script:
- **The Trigger:** A cron job that fires every minute to execute the news sweep.
- **The State (Pi Durable):** Backed by a SQLite database, meaning the agent's memory and state persist permanently. 
- **The Classifier (Jev):** Because LLM calls cost money, Jev is placed at the front of the pipeline. It handles rapid deduplication, relevance checks, and the Hype/Fact/Signal classification at a fraction of a penny per call.
- **The Reasoning Engine:** A fast, cheap LLM (like GPT-6 Luna or Haiku 5.5) does the actual text processing and summary generation.
- **Detached Clients:** The UI (whether a browser or a terminal TUI) is just a thin client. The actual agent is running independently on a local server (an M4 Mac Mini) or a cloud sandbox.

### 4. The Data Flywheel (Feedback Loop)
A proactive agent must adapt to user taste over time. Dan's agent includes a feedback loop where he can:
- Upvote/downvote specific articles.
- Send natural language feedback (e.g., *"Increase the weight of the keyword 'Gemini', and add it if it doesn't exist."*).
The agent instantly updates its SQLite configuration, ensuring the next cron execution yields higher-signal results tailored to his evolving interests.

---
*Note: This summary connects perfectly with your other notes on Harness Engineering, TypeSafe Jev cost-optimization, and Agentic Workflows. The transition to "out-of-the-loop" systems represents a major architectural shift from conversational AI to background agentic services.*
```eof
http://googleusercontent.com/youtube_content/1
