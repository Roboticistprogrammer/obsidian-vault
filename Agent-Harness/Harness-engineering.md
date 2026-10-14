---
tags: [AI, Agents, Harness-Engineering, AWS, Mike-Chambers, Production-AI, MCP, AI-Engineer]
source: https://youtu.be/gxVZ_1tuuq4?si=oNNlgLGqpy2ysV4j
channel: AI Engineer
speaker: Mike Chambers
published: September 2026
---

# Harness Engineering: Building the Production Cage for Powerful Domain Agents

## Overview: The Core Thesis
In this talk, Mike Chambers (Senior AI Specialist Developer Advocate at AWS) defines the concept of **Harness Engineering** for AI agents. 

He draws a critical distinction between "Agents that we *use*" (like coding assistants running locally on your machine) and "Agents that we *build*" (agents deployed at cloud scale for thousands of users). A "harness" is everything in an agentic system *except* the underlying LLM model. For production agents, harness engineering is the vital discipline of managing the loop, memory, state, identity, scaling, observability, and skills independently of the core model so that the agent can operate safely and autonomously at a massive scale.

## Key Concepts & Challenges

### 1. Two Types of Agents
- **Agents We Use:** Tools like Cursor, Claude Code, or local developer assistants. The harness here involves local file access, specific skill/tool configurations, and memory management tailored to one specific user. 
- **Agents We Build:** Systems designed for multi-tenant cloud deployments. The harness is much more complex, requiring serverless scaling, robust identity management, asynchronous memory, and comprehensive observability (evaluations).

### 2. What is a Harness?
If you take an AI agent and remove the Large Language Model (LLM), everything that remains—the memory manager, the tool loop, the state handling, the Model Context Protocol (MCP) server connections, the system prompts—is the **harness**. 

### 3. "No More Slop Ops"
Chambers pushes back against using agents to randomly click around cloud consoles to deploy infrastructure. Instead, agents must be constrained by harnesses to output Infrastructure as Code (IaC) to maintain proper, repeatable, and safe cloud deployments.

## Tools & Demos

### Strands Agents SDK
Chambers demonstrated building basic local agents using the **Strands Agents SDK** (an open-source, model-first framework). He showed how a few lines of Python could spin up a basic agent equipped with tools (like a calculator) and short/long-term memory session managers that rehydrate context between interactions.

### Amazon Bedrock Agent Core
To take an agent from a local machine to a multi-tenant cloud deployment, Chambers introduced **Amazon Bedrock Agent Core**.
- **CLI Deployment:** Using the `agentcore` CLI tool, developers can scaffold a complete, scalable agent in Python or TypeScript. 
- **Composability:** Agent Core separates the agent's logic from its infrastructure. You can deploy just the serverless long-term memory module, or the specific tool-loop container, and integrate it into an existing agent stack.
- **The "No-Code" Harness:** Chambers argued that ~80% of agentic use cases might simply be a good system prompt hooked to MCP tools. Using Agent Core, you can deploy a highly scalable agent using just a single configuration JSON file containing the model choice and the system prompt, entirely skipping custom application code.

---
*Note: This summary fits well with other notes on Agentic Workflows, MCP (Model Context Protocol), and AWS/Cloud Infrastructure.*
```eof

The file is complete! You can copy and paste this directly into your Obsidian vault. Let me know if you need any further summaries!
http://googleusercontent.com/youtube_content/1
