---
title: "Memory-Aware Agent Harnesses for Anti-Money Laundering"
speaker: Nacho Martinez
organization: Oracle
event: AI Engineer
source: https://www.youtube.com/watch?v=KtBivPzmOM8
duration: "51:48"
type: video-notes
topics:
  - Agent Harnesses
  - AI Agent Memory
  - Context Engineering
  - Multi-Agent Systems
  - Vector Search
  - Tool Selection
  - Anti-Money Laundering
  - Agent Evaluation
status: summarized
---

# Memory-Aware Agent Harnesses for Anti-Money Laundering

## Overview

This workshop explains how to build an autonomous AI agent by combining a language model with an execution framework, memory, tools, perception, and a control loop.

The central example is an anti-money-laundering (AML) agent that monitors financial transactions and flags suspicious activity for investigation.

The underlying engineering problem is that an LLM does not automatically possess persistent memory, continuously updated environmental awareness, or the ability to execute an extended workflow independently. These capabilities must be implemented through the surrounding system.

The workshop explores several important components:

- Agent loops and autonomous execution.
- Short-term and long-term memory.
- Episodic, semantic, and procedural memory.
- Context engineering and context compaction.
- Retrieval of relevant tools and skills.
- External storage and selective information retrieval.
- The relationship between model reasoning and the agent harness.

**Core principle:** Build a system that dynamically assembles the information and capabilities an agent needs for its current task, rather than attempting to place everything into a single prompt.

## 1. What Is an Agent Harness?

**Timestamp:** 00:12–10:48

An agent harness is the infrastructure surrounding an AI model that enables it to operate as part of a larger system.

A language model can interpret instructions and generate responses, but a production agent needs additional mechanisms to interact with its environment and complete tasks.

A harness can provide:

- Memory management.
- Tool execution.
- Perception and access to external information.
- Context construction.
- Repeated model interactions.
- Task orchestration.
- State management and recovery.

### Conceptual architecture

```text
              User / External Event
                       |
                       v
                Agent Harness
                       |
          +------------+------------+
          |            |            |
          v            v            v
       Memory         Tools      Perception
          |            |            |
          +------------+------------+
                       |
                       v
                  LLM Reasoning
                       |
                       v
                  Next Action
                       |
                       v
                Execute / Observe
                       |
                       v
                 Update State
                       |
                       +----> Repeat
```

The harness manages the interaction between the model and the rest of the system.

The model provides reasoning, while the surrounding infrastructure supplies the mechanisms required for ongoing execution.

### Why this matters

Increasing the model's intelligence does not eliminate the need for reliable software infrastructure.

An agent that must monitor financial transactions for hours needs to retrieve new records, preserve investigation state, invoke analytical tools, and determine what to do next.

Those responsibilities cannot be delegated entirely to the model's context window.

**Practical lesson:** Treat the harness as a first-class architectural component, not merely a wrapper around an LLM API.

---

## 2. Agent Loops: Turning Model Calls into Autonomous Work

An agent loop allows a system to repeatedly reason, act, observe results, and decide what to do next.

A simplified loop looks like this:

```text
Receive Task
     |
     v
Retrieve Relevant Context
     |
     v
Ask Model to Reason
     |
     v
Select an Action
     |
     v
Execute Tool / Inspect Environment
     |
     v
Evaluate the Result
     |
     +---- More Work Needed ----> Repeat
     |
     v
Complete or Escalate
```

The loop allows the agent to perform multi-step work rather than generating a single response.

### Example: AML investigation

Suppose a bank's monitoring system identifies a suspicious series of transactions.

An agent might:

1. Retrieve the relevant transaction records.
2. Identify patterns that warrant investigation.
3. Query customer and account information through authorized tools.
4. Calculate relevant financial metrics using deterministic code.
5. Compare the evidence with applicable investigation criteria.
6. Produce a report for a human analyst.

The agent should not be allowed to treat its own conclusion as proof that the investigation is complete.

The workflow needs explicit stopping conditions, evidence requirements, and escalation rules.

**Practical lesson:** Autonomy emerges from the combination of reasoning, tools, state, and repeated execution—not from the model alone.

---

## 3. Short-Term Memory vs. Long-Term Memory

**Timestamp:** 16:32 onward

One of the workshop's central topics is the distinction between information available in the current model interaction and information retained outside it.

### Short-term memory

The active context window serves as short-term working memory.

It contains the information currently available to the model, such as:

- Current instructions.
- Relevant conversation history.
- Retrieved documents.
- Tool results.
- Intermediate reasoning inputs.
- Current task state.

The context window is limited. Adding more information is not always beneficial, because irrelevant or redundant material can make it harder to focus on the important evidence.

### Long-term memory

Long-term memory stores information that may be needed across multiple interactions or over extended periods.

It can reside in external files, databases, vector indexes, or other persistent storage systems.

The workshop distinguishes three useful conceptual categories.

| Memory type | Purpose | Example |
|---|---|---|
| Episodic memory | Records of past events and experiences | Previous investigation results and the actions taken |
| Semantic memory | Facts, entities, and relationships | Customer information, account relationships, and established facts |
| Procedural memory | Skills, instructions, and reusable methods | A workflow for investigating a particular transaction pattern |

These categories represent different kinds of information, not necessarily three separate physical databases.

### Why the distinction matters

Consider an agent that investigates suspicious transactions every day.

It should not need to place every previous investigation into its active context.

Instead, it can retrieve the specific historical cases relevant to the current transaction.

Similarly, a reusable investigation procedure should be stored separately from the individual records produced during earlier investigations.

This makes it easier to update a procedure without confusing it with historical evidence.

**Practical lesson:** Separate information according to its purpose and retrieve it when needed.

---

## 4. Context Engineering and Context Compaction

**Timestamp:** Approximately 26:26 onward

The system prompt cannot contain everything the agent might need.

As an agent accumulates information from conversations, files, databases, and tool results, its working context can become unnecessarily large.

The solution is not simply to increase the amount of information passed to the model. It is to determine which information is relevant to the current task.

### Context compaction

Context compaction reduces the amount of information that must remain in the active context while preserving what is necessary for continued work.

For example, a lengthy investigation history might be summarized into:

```text
Investigation ID: AML-1042

Current objective:
Determine whether the identified transaction pattern
requires further investigation.

Completed:
- Retrieved relevant transaction records.
- Calculated transaction totals.
- Identified related accounts.

Outstanding:
- Verify the relationship between two accounts.
- Compare findings against the investigation criteria.

Evidence references:
- transaction_batch_1042
- account_relationships_1042

Status:
Pending further review.
```

The full source records remain in external storage.

The agent receives a compact status summary and can retrieve the referenced evidence when needed.

### What should be preserved?

Useful information includes:

- Current objective.
- Important decisions.
- Completed steps.
- Outstanding tasks.
- Relevant identifiers.
- Evidence references.
- Known uncertainties.
- Conditions required for completion.

Compaction should not remove evidence that is necessary to justify a consequential decision.

For financial investigations, the system should retain access to the original transaction records and preserve an auditable link between conclusions and their supporting evidence.

**Practical lesson:** Summarize working state, but keep authoritative records and supporting evidence available for retrieval.

---

## 5. Selective Retrieval of Tools and Skills

**Timestamp:** Approximately 36:39 onward

The workshop discusses dynamically retrieving tools and skills rather than including every available capability in every model request.

This is an important form of context engineering.

An agent may have access to many tools, but only a small subset will be relevant to a particular task.

Likewise, a large library of reusable skills should not automatically be injected into every interaction.

### Example architecture

```text
                   Current Task
                        |
                        v
                  Retrieval Query
                        |
             +----------+----------+
             |                     |
             v                     v
       Tool Index             Skill Index
             |                     |
             v                     v
      Relevant Tools          Relevant Skills
             |                     |
             +----------+----------+
                        |
                        v
                 Optional Reranking
                        |
                        v
                 Selected Context
                        |
                        v
                   LLM Request
```

The workshop describes a design that retrieves up to three relevant tools and three relevant skills from separate vector stores, with optional reranking to narrow the selection further.

These values are implementation choices from the discussed design, not universal optimal limits.

### Why separate tool and skill retrieval?

Tools and skills serve different purposes.

**Tools** allow an agent to perform operations or obtain information.

Examples:
- Retrieve transaction records.
- Query an account database.
- Calculate transaction totals.
- Generate an investigation report.

**Skills** provide task-specific procedures or instructions.

Examples:
- How to investigate a particular transaction pattern.
- How to structure an investigation report.
- Which checks must be completed before escalation.

A tool provides a capability; a skill guides the agent in using capabilities appropriately.

### Why use vector search?

Vector search can retrieve tools or skills based on semantic similarity between the current task and indexed descriptions.

For example, a request to investigate repeated transfers between related accounts might retrieve a skill concerning linked-account transaction analysis.

However, semantic similarity alone does not guarantee that a tool is appropriate or authorized.

The system should also validate permissions, input requirements, and operational constraints.

### Why use reranking?

Initial retrieval may return several plausible candidates.

A reranking stage can evaluate those candidates more carefully and prioritize the most relevant ones.

This can reduce irrelevant context, although it introduces additional computation and latency.

**Practical lesson:** Retrieve the capabilities relevant to the current task, then validate that they are appropriate before providing them to the agent.

---

## 6. Dynamic Context Assembly

Selective retrieval leads to a broader architectural pattern: the agent's context can be constructed dynamically for each model call.

Instead of using one static prompt containing every instruction, tool, skill, and historical fact, the harness assembles a task-specific context.

A request might contain:

```text
SYSTEM INSTRUCTIONS
- Role and operating constraints.
- Security and authorization requirements.

CURRENT TASK
- Investigate transaction pattern AML-1042.

RETRIEVED FACTS
- Relevant transaction records.
- Relevant account relationships.

SELECTED TOOLS
- Transaction lookup.
- Account relationship query.
- Financial calculation.

SELECTED SKILLS
- Transaction pattern investigation.
- Evidence-based report preparation.

TASK STATE
- Completed checks.
- Outstanding questions.
- References to previous results.

EXPECTED OUTPUT
- Findings.
- Supporting evidence.
- Uncertainties.
- Recommended next step.
```

The contents should change according to the current task.

An account lookup does not need the same context as a final investigation report.

### The resulting principle

The objective is not to maximize the amount of information available in a single prompt.

It is to maximize the relevance of the information provided for the current decision.

**Practical lesson:** Build context as a task-specific package rather than treating the system prompt as the permanent home for all agent knowledge.

---

# The AML Example: An Agent for Suspicious Transactions

The financial investigation scenario provides a concrete way to understand why an agent harness needs memory and controlled tool use.

Consider an illustrative system that flags suspicious transactions for human review.

## Step 1: Receive a Transaction Alert

An external monitoring system generates an alert.

The alert contains an identifier and enough initial information to begin an investigation.

The agent should not assume the alert is correct simply because it was generated by another system.

## Step 2: Retrieve Relevant Records

The harness queries authorized data sources for the transaction records, account information, and any other evidence needed for the investigation.

It retrieves only the relevant subset rather than loading the entire database into the model's context.

## Step 3: Select Tools and Skills

The retrieval system selects suitable capabilities and procedures.

For example:
- Transaction history lookup.
- Account relationship analysis.
- Deterministic financial calculations.
- An applicable investigation procedure.

## Step 4: Analyze the Evidence

The model interprets the retrieved evidence and identifies questions requiring further investigation.

Deterministic calculations should be performed by appropriate software rather than relying on the LLM to perform every numerical operation mentally.

## Step 5: Preserve Investigation State

The harness records which checks have been completed, which records support the findings, and which questions remain unresolved.

This allows the investigation to continue without keeping its entire history in the active context.

## Step 6: Produce a Reviewable Result

The agent produces a report containing:
- The alert being investigated.
- Relevant transaction evidence.
- Calculated results.
- Findings and uncertainties.
- References to supporting records.
- Recommended next steps.

## Step 7: Escalate When Necessary

If evidence is incomplete or the case meets defined escalation criteria, the agent routes the investigation to a qualified human reviewer.

The system should not autonomously freeze accounts, accuse customers of criminal conduct, or take other consequential actions solely on the basis of an unverified LLM conclusion.

### Workflow diagram

```text
Transaction Alert
       |
       v
Retrieve Relevant Records
       |
       v
Retrieve Tools and Skills
       |
       v
Construct Task-Specific Context
       |
       v
Analyze Evidence
       |
       v
Execute Required Checks
       |
       v
Save Investigation State
       |
       v
Evaluate Completion Criteria
       |
       +---- Incomplete ----> Retrieve More Evidence
       |                            |
       |                            v
       |                       Continue Loop
       |
       v
Produce Evidence-Based Report
       |
       v
Human Review / Escalation
```

This diagram is a conceptual application of the workshop's architecture.

---

# Main Takeaways

1. **The harness is as important as the model.** Memory, tools, perception, and execution control turn model capabilities into an operational agent.

2. **Short-term and long-term memory serve different purposes.** The active context contains what the model needs now; external memory preserves information for later retrieval.

3. **Distinguish episodic, semantic, and procedural memory.** Historical experiences, facts, and reusable methods should not be treated as interchangeable information.

4. **Context compaction helps long-running tasks.** Preserve the current state and important references without keeping every previous interaction in the active context.

5. **Retrieve tools and skills dynamically.** Provide the capabilities relevant to the current task rather than exposing the model to an unnecessarily large collection.

6. **Use retrieval as part of context engineering.** Search, filtering, and optional reranking help determine which information enters a model request.

7. **Keep evidence accessible.** A compact summary should point back to authoritative records rather than replace them.

8. **Combine model reasoning with deterministic operations.** Financial calculations, data validation, and authorization checks should use appropriate software controls.

9. **Preserve state across extended workflows.** An agent needs a reliable way to determine what has happened and what remains to be done.

10. **Design for auditability.** In sensitive domains such as financial crime investigation, conclusions should be traceable to supporting evidence and subject to appropriate human oversight.

---

# Application to My Agentic WMS

The ideas from this workshop are directly relevant to designing an agentic Warehouse Management System that connects inventory software with autonomous drone operations.

## 1. Separate the Three Types of Memory

A warehouse agent can use the same conceptual distinction.

| Memory type | WMS example | Possible storage |
|---|---|---|
| Episodic | Previous cycle-count results, discrepancy investigations, and completed tasks | Relational database or event store |
| Semantic | Warehouse hierarchy, SKU information, location relationships, and operational facts | Relational database, knowledge graph, or other structured store |
| Procedural | Cycle-count procedure, discrepancy investigation workflow, and putaway rules | Version-controlled instructions, skill repository, or workflow definitions |

These are logical categories. One physical database may store several types, and the best storage choice depends on access patterns and consistency requirements.

### Why this matters

Suppose a drone identifies a possible discrepancy in a warehouse rack.

The agent may need:
- **Semantic memory:** Which SKU should occupy the location?
- **Episodic memory:** What happened during the previous count?
- **Procedural memory:** How should this discrepancy be investigated?

Combining these sources can help the agent interpret the observation without confusing current inventory facts with historical events or operating procedures.

## 2. Build a Context Retrieval Layer

A warehouse system should not pass the entire inventory database, every agent instruction, and all historical observations into the LLM.

Instead, retrieve information based on the current task.

For a discrepancy investigation:

```text
Drone Observation
       |
       v
Identify Warehouse Location
       |
       v
Retrieve Expected Inventory
       |
       v
Retrieve Relevant History
       |
       v
Select Investigation Skill
       |
       v
Construct Agent Context
       |
       v
Analyze and Validate
```

For a space-utilization query, the retrieval policy would be different: it might prioritize rack capacity, storage constraints, SKU dimensions, and current occupancy.

This allows multiple agents to use the same underlying warehouse data while receiving different task-specific contexts.

## 3. Distinguish Facts from Recommendations

Your Warehouse Advisor Agent should not treat model-generated statements as authoritative inventory records.

A useful architecture separates:

- **Authoritative data:** Inventory quantities, SKU identifiers, warehouse locations, and recorded movements.
- **Historical evidence:** Previous counts, observations, and discrepancy investigations.
- **Procedural knowledge:** Warehouse rules and approved operating procedures.
- **Agent conclusions:** Interpretations, recommendations, and hypotheses.

This distinction is particularly important when combining structured inventory data with a knowledge graph or vector database.

A vector search result may help locate relevant information, but it should not override an authoritative stock record without an explicit reconciliation process.

## 4. Retrieve Drone Capabilities According to the Task

Not every warehouse agent needs access to every drone capability.

For example:

| Task | Relevant capabilities |
|---|---|
| Answer an inventory question | Inventory query and record lookup |
| Investigate a discrepancy | Inventory lookup, observation retrieval, reconciliation, evidence recording |
| Analyze space utilization | Capacity query, occupancy analysis, constraint checking |
| Request a drone inspection | Mission planning interface, availability check, authorization and safety validation |

The LLM should not receive unrestricted access to physical controls simply because it can generate tool calls.

Drone missions should be subject to authorization, geofencing, collision avoidance, battery constraints, and other appropriate robotics safety mechanisms.

## 5. Connect Memory to Guardrails

A memory-aware agent also needs to distinguish between information it can use and actions it is permitted to perform.

For example, historical evidence might indicate that a particular rack has repeatedly produced unreliable visual counts.

That information could trigger additional verification, but it should not automatically authorize an inventory adjustment.

A guardrails layer can enforce rules such as:

- Inventory changes require appropriate validation.
- Uncertain visual observations trigger a recount or human review.
- Drone missions must satisfy operational safety constraints.
- Agents can access only the data and tools required for their responsibilities.
- Consequential actions must be logged and traceable.

**The key principle is that memory informs decisions; it does not grant authorization.**

## 6. Evaluate the Architecture Before Adding More Agents

For your proof of concept, I would compare three implementations:

1. **Baseline:** A single agent with direct access to a small, structured set of warehouse data.
2. **Memory-aware agent:** The same agent with selective retrieval of historical records and domain-specific procedures.
3. **Multi-agent architecture:** Specialized advisor, discrepancy, and space-utilization agents coordinated through an orchestrator.

Evaluate them using the same tasks and datasets.

Useful measurements include:
- Inventory-answer accuracy.
- Discrepancy detection precision and recall.
- Retrieval relevance.
- Incorrect or unsupported claims.
- End-to-end latency.
- Cost per completed task.
- Human intervention rate.
- Correct handling of unauthorized or unsafe requests.

This will help establish whether memory and multi-agent orchestration deliver measurable benefits over a simpler implementation.

---

# Suggested Architecture for Further Exploration

For your platform, the most reusable idea is a shared context and memory service.

```text
                Warehouse Event / User Query
                           |
                           v
                    Task Orchestrator
                           |
                           v
                    Context Retrieval
                           |
              +------------+------------+
              |            |            |
              v            v            v
         Structured     Historical    Skills and
         WMS Data        Records      Procedures
              |            |            |
              +------------+------------+
                           |
                           v
                  Context Construction
                           |
                           v
                    Specialized Agent
                           |
                           v
                   Validation Layer
                           |
              +------------+------------+
              |                         |
              v                         v
       Read-Only Answer          Proposed Action
                                        |
                                        v
                               Authorization Check
                                        |
                                        v
                               Execute / Escalate
```

The advantage of this design is that different agents can share a consistent retrieval and validation infrastructure without needing identical prompts, tools, or memory policies.

It also creates a clear place to enforce access permissions, preserve evidence references, and audit proposed actions.

---

# Questions for Further Exploration

- How should episodic, semantic, and procedural memory be represented in a production agent?
- When should the system use a relational database, vector database, or knowledge graph?
- How can retrieved information be checked for freshness and authority?
- How should the system decide which tools and skills are relevant to a particular task?
- When is reranking worth its additional latency and cost?
- How can an agent resume a long-running task without repeating completed work?
- How can memory retrieval be evaluated independently of the model's final answer?
- How should an agent distinguish an observed fact from a hypothesis?
- What evidence must be preserved before an inventory record can be changed?
- How should MCP-based tool interfaces be combined with authorization and robotics safety controls?

---

# Related Obsidian Notes

- [[AI Agents]]
- [[Agent Harnesses]]
- [[Agentic WMS Architecture]]
- [[Context Engineering]]
- [[AI Agent Memory]]
- [[Episodic Memory]]
- [[Semantic Memory]]
- [[Procedural Memory]]
- [[Vector Databases]]
- [[Knowledge Graphs]]
- [[Retrieval-Augmented Generation]]
- [[Agent Orchestration]]
- [[Agent Security and Guardrails]]
- [[Model Context Protocol (MCP)]]
- [[Autonomous Warehouse Robotics]]
- [[Digital Twins]]

---

# Source

**Video:** Memory-Aware Agent Harnesses for Anti-Money Laundering  
**Speaker:** Nacho Martinez, Oracle  
**Event:** AI Engineer  
**URL:** https://www.youtube.com/watch?v=KtBivPzmOM8

*Note: The summary follows the workshop's transcript-derived concepts and reported architecture. The WMS examples are proposed applications of those concepts, not demonstrations from the original presentation.*
