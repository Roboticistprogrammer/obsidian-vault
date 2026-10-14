---
tags: [AI, Agents, CLI, MCP, Airbyte, Tool-Design, AI-Engineer]
source: https://youtu.be/3wj6sgbi1YA?si=qbxuPN9Y_JGxl-A-
channel: AI Engineer
speaker: Pedro Lopez
published: October 2026
---

# Designing CLIs for Agents, Not Humans

## Overview: The Core Thesis
In this talk, Pedro Lopez (Software Engineer at Airbyte) explores the architectural shift required when designing tools and interfaces meant to be used by autonomous AI agents rather than human developers. 

Airbyte evolved from an open-source data movement tool into a "Data Action Layer" for AI agents, allowing LLMs to seamlessly query and join data across tools like Zendesk, Stripe, and GitHub via a Context Store. To expose this functionality to AI, Airbyte built two distinct interfaces: an **MCP (Model Context Protocol) Server** and an **Agentic CLI**. Lopez shares the critical design lessons learned, emphasizing that "developer-friendly" features for humans are often roadblocks for AI agents.

## Key Concepts & Challenges

### 1. Designing the MCP (Model Context Protocol) Server
MCPs are ideal for non-technical users wanting to easily plug their data directly into consumer UIs like ChatGPT or Claude.
- **Keep Tool Count Narrow:** Instead of exposing a separate tool for every possible API action, use *progressive discovery*. Expose just two core tools (e.g., `describe_connector` to fetch the schema, and `execute` to run the query). This prevents context-window bloat and dramatically improves agent performance.
- **OAuth is Mandatory:** Agents need long-lived, offline sessions. Avoid simple bearer tokens. Use robust OAuth flows because MCP users expect the connection to work continuously in the background like a mobile app.
- **Client Fragmentation:** The MCP specification is not uniformly supported across clients. For instance, while URL-mode "elicitation" (prompting the user for input via a UI widget) is in the spec, clients like Claude Desktop do not fully support it yet, forcing developers to build custom workarounds.

### 2. Designing the CLI for Agents
CLIs are the "power-user" tool for agents, allowing for long-running tasks and massive data processing by leveraging Unix pipes (e.g., sending output to `jq` or files). To design a CLI for an agent:
- **JSON In, JSON Out:** Do not design CLIs with traditional human-friendly flags (e.g., `-u`, `-p`, `--force`). Agents are inherently native to JSON. Accept complex JSON payloads as inputs and exclusively output JSON so agents (and pipeline tools) can parse the results effortlessly.
- **No Interactive Prompts:** If a human CLI asks "Are you sure? [y/n]", an AI agent will freeze indefinitely waiting for the prompt. All authentication and configurations must be handled headlessly via environment variables or config files.
- **Strict Noun-Verb Consistency:** Keep commands highly predictable (`connector list`, `workspace list`). If agents consistently hallucinate a command, *make that hallucination the actual command contract*. It means your original naming convention was unintuitive to the LLM.
- **Ship with Skills:** A CLI is useless to an agent if the agent doesn't know it exists. Always ship CLI packages with agent "skills" (manifest files) that progressively load descriptions into the agent's context, teaching it exactly how and when to invoke the tool.

### 3. MCP vs. CLI: Which is Better?
- **MCP Limitations:** Hosted MCPs are always up-to-date, but they suffer from strict constraints imposed by clients (e.g., Claude restricts tool descriptions to 2KB and enforces strict execution timeouts). They are best for quick prototyping and ad-hoc reporting.
- **CLI Limitations:** CLIs do not have timeouts or payload limits, making them perfect for heavy workloads. However, they suffer from version rot (requiring package updates) and the lack of guardrails means a misconfigured agent can get stuck in infinite execution loops.

---
*Note: This summary fits perfectly alongside your existing vault notes regarding Harness Engineering, Tool Usage, and Model Context Protocol architectures.*
```eof
http://googleusercontent.com/youtube_content/1
