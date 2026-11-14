---
title: "Build Your Own Team of Managed Agents"
speaker: Jack Wotherspoon
organization: Google DeepMind
source: https://www.youtube.com/watch?v=Y_p3y6zP5QE
duration: "19:43"
type: video-notes
topics:
  - AI Agents
  - Multi-Agent Systems
  - Gemini
  - Agent Orchestration
  - Cloud Sandboxes
  - AI Engineering
status: summarized
---

# Build Your Own Team of Managed Agents

## Overview

In this talk, Jack Wotherspoon from Google DeepMind introduces **Gemini Managed Agents**, an approach to building and operating AI agents in managed cloud environments.

Instead of running agents directly on a developer's computer, the system allows developers to launch agents through a single API call and execute their work inside cloud sandboxes. These environments provide the infrastructure needed for agents to perform longer-running tasks, access tools, share context, and collaborate with other agents.

The talk explores the architecture behind managed agents, including the Interactions API, the Antigravity agent harness, shared execution environments, reusable skills, secure credential handling, scheduled execution, and multi-agent workflows.

## Key Concepts

### 1. Managed Agents and Cloud Sandboxes

Managed agents execute inside dedicated cloud environments rather than relying exclusively on a developer's local machine.

Potential advantages include:

- **Isolation:** Execute agent tasks in controlled environments.
- **Scalability:** Launch agents without manually managing every execution environment.
- **Persistence:** Support longer-running tasks that continue beyond a single interaction.
- **Automation:** Trigger agent workflows through APIs and scheduled jobs.
- **Reproducibility:** Standardize the environment in which agents perform their work.

The underlying idea is to treat an agent as an independently executing software process rather than merely a chatbot that responds to individual prompts.

### 2. The Interactions API

The Interactions API provides an interface for interacting with models and agents.

In the managed-agent architecture, the API serves as an entry point for initiating and coordinating agent work.

This makes it possible to integrate agent execution into larger software systems without manually managing every step of the interaction.

### 3. Agent Harnesses

An agent harness provides the execution infrastructure and supporting mechanisms that allow an AI model to perform useful work.

The talk introduces the Antigravity harness in the context of Gemini Managed Agents.

A harness can bring together:
- Model interactions.
- Tool execution.
- Agent instructions.
- Execution environments.
- Task orchestration.
- Access to external resources.

The distinction is important: **the model supplies reasoning capabilities, while the harness provides the operational structure through which the agent acts.**

### 4. Shared Environments and Multi-Agent Collaboration

Multiple agents can participate in workflows where they share access to an execution environment.

This enables tasks to be divided between specialized agents instead of requiring one agent to complete every stage.

For example, a workflow might involve:
1. A research agent collecting information.
2. An analysis agent interpreting the collected data.
3. A specialized agent performing quantitative calculations.
4. A final agent consolidating the results.

Shared environments can help agents exchange artifacts and build on each other's work.

However, shared access also introduces engineering considerations, including coordination, conflicting modifications, permissions, and the reliability of intermediate results.

### 5. Reusable Skills and Agent Instructions

The system supports uploaded skills and `AGENTS.md` files.

These provide a way to supply agents with reusable instructions and task-specific knowledge.

Rather than embedding every operational detail in each prompt, developers can organize instructions and capabilities into reusable components.

This approach can help standardize how agents perform recurring tasks across different workflows.

### 6. Secure Credential Handling

Agents operating in cloud environments may need to interact with external services that require authentication.

Exposing API keys, passwords, or other credentials directly to an agent's execution environment creates security risks.

The talk describes a proxy mechanism designed to keep secrets out of the sandbox.

This illustrates an important principle for production agent systems:

**Agents should receive the capabilities they need without automatically receiving unrestricted access to the underlying credentials.**

Security must be considered at the infrastructure level, not just through prompt instructions.

### 7. Scheduled and Multi-Agent Workflows

Managed agents can support scheduled triggers and workflows involving multiple agents.

This expands their potential applications beyond interactive assistance.

An agent system could be configured to perform recurring tasks, initiate research, process information, or coordinate a sequence of specialized activities.

The larger architectural shift is from executing individual prompts to operating persistent, automated workflows.

## Demonstration: Research Agent and Quantitative Analysis

One demonstration involves an agent researching SEC filings and handing its work to a quantitative analysis agent.

The workflow illustrates how agents can divide a complex task into specialized responsibilities.

A simplified representation is:

```text
Task Request
     |
     v
Research Agent
     |
     v
SEC Filing Research
     |
     v
Research Results / Artifacts
     |
     v
Quantitative Analysis Agent
     |
     v
Analysis Results
```

The research agent focuses on gathering relevant information, while the quantitative agent performs a different type of analysis.

This separation allows each agent to concentrate on a narrower responsibility.

It also demonstrates why multi-agent systems need more than multiple model calls: they need mechanisms for passing results between agents, managing execution, and controlling access to resources.

## Main Takeaways

1. **Agents need infrastructure, not just intelligence.** Reliable execution environments, tools, and orchestration are essential for turning model capabilities into useful software.

2. **Cloud execution changes the development model.** Agents can operate independently of a developer's local computer and support longer-running workflows.

3. **Specialization enables collaboration.** Different agents can handle research, analysis, and other distinct subtasks within one workflow.

4. **Reusable instructions improve consistency.** Skills and files such as `AGENTS.md` provide mechanisms for standardizing agent behavior.

5. **Security must be built into the architecture.** Credential isolation and controlled access are important when agents execute code or interact with external services.

6. **Multi-agent systems require coordination.** Sharing an environment and passing results between agents introduces challenges involving state, permissions, execution, and reliability.

7. **APIs make agent capabilities composable.** Managed execution can be integrated into broader applications and automated processes.

## Applications to My Work

### Agentic Warehouse Management System

The managed-agent architecture provides several ideas that could be applied to an agentic Warehouse Management System (WMS).

Potential agent roles include:

- **Warehouse Advisor Agent:** Answers operational questions using inventory and warehouse data.
- **Discrepancy Detection Agent:** Investigates differences between recorded inventory and observed warehouse conditions.
- **Space Utilization Agent:** Analyzes storage capacity and recommends improvements.
- **Inventory Analysis Agent:** Examines stock movements and identifies unusual patterns.

A managed execution architecture could allow these agents to run independently while coordinating through shared data and controlled interfaces.

For example:

```text
Warehouse Event
      |
      v
Orchestration Layer
      |
      +----> Inventory Agent
      |
      +----> Discrepancy Agent
      |
      +----> Space Utilization Agent
                    |
                    v
              Shared Results
                    |
                    v
             WMS / Human Review
```

This is a proposed application of the concepts, rather than a system specifically demonstrated in the talk.

### Important Architectural Considerations

For a warehouse environment, the execution model would need additional safeguards:

- Agents should retrieve inventory information through controlled APIs.
- Agents should not automatically receive unrestricted access to warehouse databases or hardware.
- Changes to inventory records should be validated before being committed.
- Physical actions involving autonomous drones should pass through a dedicated robotics interface with safety constraints.
- Long-running tasks should have timeouts, resource limits, and clear failure-handling procedures.
- Agent outputs should be traceable so warehouse operators can understand why a recommendation was produced.

Cloud-managed agents may be suitable for research, planning, reporting, and coordination. Time-critical drone control and safety-critical navigation should remain within appropriate robotics and control systems rather than depending on a remote LLM execution loop.

## Questions for Further Exploration

- How does Gemini Managed Agents manage persistent state across long-running tasks?
- How are shared environments isolated when multiple agents execute concurrently?
- What mechanisms prevent one agent from corrupting another agent's files or intermediate results?
- How should agents authenticate with external services without exposing secrets?
- How can workflows recover when one agent fails or returns an incorrect result?
- How do managed agents compare with self-hosted agent frameworks?
- Could a similar architecture support an MCP-based interface for a warehouse's software agents and robotics systems?

## Related Topics

- [[AI Agents]]
- [[Multi-Agent Systems]]
- [[Agent Orchestration]]
- [[LLM Agent Harnesses]]
- [[Cloud Sandboxes]]
- [[Agent Security and Guardrails]]
- [[Model Context Protocol (MCP)]]
- [[Agent-to-Agent Communication]]
- [[Agentic WMS Architecture]]
- [[Autonomous Warehouse Robotics]]

## Source

[Build Your Own Team of Managed Agents — Jack Wotherspoon, Google DeepMind](https://www.youtube.com/watch?v=Y_p3y6zP5QE)

*Note: These notes summarize the available video description and its reported demonstrations. They are not a verified, timestamp-by-timestamp transcript of the complete talk.*
