---
title: "Claude Changed Completely — Every NEW Concept Explained"
speaker: Simon Scrapes
source: https://www.youtube.com/watch?v=YAuWBqZ_gjs
channel: Simon Scrapes
duration: "19 min"
type: video-notes
topics:
  - AI Agents
  - Claude Code
  - Coding Agents
  - Agent Skills
  - Multi-Agent Systems
  - Context Engineering
  - Workflow Automation
  - Agent Verification
status: summarized
---

# Claude Changed Completely — Every NEW Concept Explained

## Overview

This video explores how advances in coding agents are changing the way developers should build and manage agentic workflows.

The speaker argues that techniques that were useful when models were less capable may now introduce unnecessary complexity. Examples include writing extensive skills for basic tasks, switching models excessively, and relying on dedicated planning modes when the model can already plan effectively.

However, this does not mean that every workflow component is obsolete.

Planning can still help humans review decisions before implementation. Skills remain useful when they encode corrections to recurring mistakes. Delegation can improve complex workflows, while hooks and explicit completion conditions can help verify results.

The central principle is:

**Build workflows around the model's actual limitations, not around assumptions about what the model cannot do.**

Rather than adding more instructions and agents by default, establish what the model can already accomplish, identify where it fails, and introduce only the context, tools, and controls needed to address those failures.

## 1. Skills Are Becoming Corrections Rather Than Complete Procedures

**Timestamp:** 00:59

### The traditional approach

Previously, developers often created detailed agent skills containing step-by-step procedures for performing common tasks.

A writing skill, for example, might include extensive instructions about:
- Tone and style.
- Sentence structure.
- Output formatting.
- Editing procedures.
- Quality standards.

The intention was to compensate for weaknesses in the model by explicitly describing the entire process.

### The newer approach

As models improve, many of these instructions may become redundant.

The speaker recommends a different process:

1. Ask the model to perform the task without a custom skill.
2. Evaluate the result against explicit quality criteria.
3. Identify recurring mistakes or missing requirements.
4. Add instructions that address those specific problems.
5. Compare the results with and without the skill.
6. Retain the skill only when it demonstrably improves performance.

This turns a skill from a complete standard operating procedure into a collection of targeted corrections.

### Skill evaluation

A useful evaluation compares two conditions:

| Condition | Purpose |
|---|---|
| Without skill | Establish the model's baseline capability |
| With skill | Determine whether the additional instructions improve performance |

Both outputs should be evaluated against the same criteria.

If the skill does not improve the result, it may be unnecessary.

If only a small portion helps, remove the redundant instructions and retain the useful corrections.

### Important qualification

This approach does not imply that every skill should be deleted.

Skills may remain valuable when they encode specialized domain knowledge, organizational requirements, proprietary procedures, or constraints that the model would not otherwise know.

**Practical lesson:** Evaluate skills empirically rather than assuming that more instructions produce better results.

---

## 2. Model Selection: Capability, Effort, and Cost

**Related topic:** Model routing

The video discusses the trade-off between selecting a capable model for a task and trying to minimize the cost of every individual interaction.

A weaker model may appear cheaper per request but require additional attempts, corrections, and tool calls to complete the same task.

The total cost of a workflow depends on more than the price of an individual request.

Relevant factors include:
- Number of model calls.
- Reasoning effort.
- Tool usage.
- Retry frequency.
- Task completion time.
- Quality of the final result.

### A practical strategy

Choose a model that is sufficiently capable for the task, then adjust the reasoning effort to match its difficulty.

For example:

| Task | Suggested strategy |
|---|---|
| Simple text transformation | Use a lightweight, efficient workflow |
| Routine coding change | Use a capable coding model with moderate effort |
| Complex architectural decision | Use a stronger reasoning configuration |
| Persistent debugging failure | Escalate reasoning or request a second opinion |
| Safety-critical implementation | Require independent verification regardless of model capability |

The specific model choice depends on the tools, pricing, and capabilities available at the time.

### Advisor models

The speaker also discusses the idea of using an advisor model to provide additional reasoning when the primary model encounters a difficult decision.

The general workflow is:

1. A primary model handles the task.
2. A difficult decision or unresolved problem arises.
3. An advisor model provides additional guidance.
4. The primary model continues the task.

This can be useful when a stronger model is needed only for selected decisions.

However, introducing a second model also adds orchestration complexity and additional token consumption.

**Practical lesson:** Optimize for the cost and reliability of the completed task, not merely the cost of an individual model call.

---

## 3. Planning Is Still Valuable for Humans

**Timestamp:** 06:54

One of the most interesting distinctions in the video concerns planning modes.

The speaker discusses the argument that increasingly capable models may not need a dedicated plan mode to reason through their work.

However, he highlights a different reason to plan: human oversight.

A plan is not merely an instruction telling the model what to do. It is also an artifact that lets a developer inspect the proposed changes before they happen.

### Why planning remains useful

For a complex software task, a written plan can establish:

- The intended outcome.
- The files and components likely to change.
- Architectural constraints.
- Dependencies.
- Potential risks.
- Acceptance criteria.
- Conditions that must be satisfied before completion.

This becomes especially important when a task involves multiple files, external systems, or significant architectural decisions.

### Small tasks versus large tasks

**Small task:**

A concise instruction with a clear goal and a few constraints may be sufficient.

**Large task:**

A structured plan, explicit acceptance criteria, and a reviewable implementation strategy may be more appropriate.

For example, changing the wording of a dashboard label probably does not require a lengthy planning document.

Redesigning an inventory data model and migrating existing records warrants much more preparation.

### The key distinction

The value of planning should be assessed from two perspectives:

1. Does the model need a separate planning procedure to complete the task?
2. Does the human need a reviewable plan to understand and control the work?

These are different questions.

**Practical lesson:** Keep planning proportional to task complexity and risk. Even if a model can plan internally, a visible plan may still be valuable for human supervision.

---

## 4. Goal-Based Execution and Explicit Completion Criteria

**Timestamp:** Approximately 11:42–13:36

The video discusses goal-oriented execution, including the `/goal` mechanism in Claude Code.

The purpose of this approach is to let an agent continue working toward a defined outcome without requiring the user to repeatedly instruct it to continue.

The important engineering question is how the system determines that the task is actually finished.

### The problem with vague goals

Consider this instruction:

```text
Goal: Finish the inventory report.
```

What does "finish" mean?

The report might exist but contain incorrect calculations, missing records, or invalid formatting.

If completion is determined mainly from the conversation, the system may accept the agent's own claim that the work is finished without independently verifying the artifact.

### Define observable completion conditions

A better goal describes the evidence required for completion.

For example:

```text
Goal:
Generate the inventory discrepancy report.

Definition of done:
- The report file exists.
- All required warehouse records are included.
- The validation script passes.
- The validation output is available for review.
- Any unresolved discrepancies are explicitly reported.
```

This makes the goal easier to evaluate.

### Deterministic checks

Whenever possible, use a test or validation procedure that produces an observable result.

Examples include:
- Unit tests passing.
- A schema validation succeeding.
- A generated file being checked for required fields.
- A database migration completing successfully.
- An API response matching an expected structure.
- A report reconciliation producing zero unexplained differences.

A completion checker that only reads a conversation cannot independently establish that a file or external system is correct.

The exact capabilities of any particular checker depend on its implementation.

**Practical lesson:** Define completion in terms of verifiable outcomes, not statements made by the agent.

---

## 5. Subagents and Forked Conversations

**Timestamp:** Approximately 11:42 onward

The video discusses different approaches to delegating work, including subagents and forked conversations.

These approaches address a central problem in multi-agent systems: how to give another agent the information it needs without unnecessarily expanding the main conversation.

### Subagents

A subagent handles a narrower task and returns its result to the primary agent.

A useful division of responsibilities might look like this:

```text
Primary Agent
     |
     +----> Research Agent
     |          |
     |          +----> Research Results
     |
     +----> Analysis Agent
     |          |
     |          +----> Analysis Results
     |
     +----> Testing Agent
                |
                +----> Test Results
     |
     v
Primary Agent Integrates Results
```

The primary agent receives the relevant outputs without necessarily needing every detail of each subagent's internal execution.

Potential advantages include:
- Reduced clutter in the primary context.
- Clearer task boundaries.
- Specialized execution.
- Easier separation of responsibilities.

### Forked conversations

A forked conversation starts from an existing conversation state.

Unlike a narrowly scoped subagent, a fork can inherit the earlier context needed to continue a related task.

This may be useful when a secondary task depends on decisions or information established earlier.

### Choosing between them

| Requirement | Suitable approach |
|---|---|
| A self-contained research task | Subagent |
| An independent validation task | Subagent |
| A related task requiring previous conversation context | Fork |
| A complex workflow with several distinct stages | Orchestration with multiple specialized tasks |
| A task requiring shared state or coordinated modifications | Explicit coordination and conflict management |

These are general design guidelines, not universal rules for every agent framework.

### A crucial limitation

Delegation does not automatically make a workflow more reliable.

It introduces additional questions:
- Which context should be shared?
- Which results should be trusted?
- How should failures be handled?
- Can two agents modify the same resource?
- How should intermediate artifacts be passed between tasks?

**Practical lesson:** Delegate based on task boundaries and context requirements, rather than spawning agents simply because the framework supports them.

---

## 6. Dynamic Workflows and Multi-Agent Research

**Timestamp:** Approximately 13:36 onward

The video discusses more structured agent workflows, including dynamic workflows associated with complex research tasks.

The underlying idea is that some problems require more than one or two independent subagents.

They benefit from a workflow that specifies how specialized agents interact and how their outputs are combined.

### Six general workflow patterns

The source material describes six patterns:

**1. Classify and Act**

Classify an incoming request and route it to an appropriate action or specialized agent.

Example: Route an inventory question to the inventory agent and a location discrepancy to the discrepancy investigation workflow.

**2. Fan-Out and Synthesize**

Split a problem into several independent subtasks, execute them separately, and combine the results.

Example: Investigate inventory discrepancies across several warehouse zones concurrently, then consolidate the findings.

**3. Adversarial Verification**

Use an independent reviewer to challenge or verify a result.

Example: One agent proposes a stock reconciliation; another checks the calculations and searches for unsupported assumptions.

**4. Generate and Filter**

Generate multiple candidate solutions and filter them according to explicit criteria.

Example: Produce several candidate warehouse slotting arrangements and reject those that violate storage constraints.

**5. Tournament**

Compare candidate solutions against each other using a defined evaluation process.

Example: Evaluate alternative replenishment strategies using the same cost, distance, and capacity metrics.

**6. Loop Until Done**

Repeat execution and evaluation until a defined completion condition is satisfied.

Example: Correct a data transformation until the validation suite passes or a retry limit is reached.

### Important distinction

These patterns are general workflow architectures. They should not be interpreted as evidence that every framework automatically implements all six patterns or that every task benefits from them.

### Managing complexity

The speaker cautions that multi-agent research can become expensive and difficult to control if too many agents are spawned.

A reasonable strategy is to start with a small number of agents, measure whether parallel execution improves the result, and expand only when necessary.

**Practical lesson:** Design the coordination structure around the problem. More agents are not inherently better.

---

## 7. Hooks as Verification and Control Mechanisms

**Timestamp:** Approximately 13:36–16:35

The video also discusses hooks that can intervene in an agent's workflow.

Conceptually, a hook provides a mechanism for running a check or action when a specified event occurs.

For example, a verification hook might run when the agent attempts to finish a task.

### Prompt-based checks

A prompt-based check evaluates information available in the conversation.

It can be useful for checking whether the agent has addressed required questions or provided expected explanations.

However, it may not independently inspect files or execute tests.

### Agent-based checks

An agent-based check can potentially inspect additional resources, such as project files, depending on the framework's capabilities and permissions.

This can support more substantive verification.

For example, a reviewer could inspect a generated report alongside a reference document and identify violations of specified requirements.

The exact distinction between hook types is framework-specific, and the video discusses experimental capabilities that may change.

### Why hooks matter

Without a verification step, a workflow may terminate as soon as the model believes its work is complete.

With an appropriate check, completion can depend on an additional review.

For example:

```text
Agent Produces Report
        |
        v
Completion Hook
        |
        v
Check Report Against Requirements
        |
        +---- Pass ----> Allow Completion
        |
        +---- Fail ----> Return Findings to Agent
                              |
                              v
                        Revise Report
                              |
                              v
                         Check Again
```

### Limits of verification

An LLM-based reviewer can still make mistakes.

Where possible, combine model-based review with deterministic checks.

For example:
- Use a script to validate required fields.
- Use tests to check software behavior.
- Use a model to assess ambiguous requirements.
- Require human approval for consequential changes.

**Practical lesson:** Use the cheapest reliable verification mechanism for each requirement, and do not confuse a plausible explanation with proof of correctness.

---

## 8. Event-Driven Automation and Long-Running Tasks

**Timestamp:** Approximately 16:35 onward

The video discusses the difference between scheduled execution and event-driven execution.

### Scheduled execution

A task starts at a specified time or according to a recurring schedule.

Examples:
- Generate a daily warehouse report.
- Review inventory every morning.
- Produce a weekly summary.

### Event-driven execution

A task starts when a particular event occurs.

Examples:
- A new inventory discrepancy is detected.
- A warehouse integration reports a failed synchronization.
- A new document is uploaded.
- A test fails after a code change.

The distinction is important because many operational workflows are triggered by events rather than time.

### Long-running tasks and resource limits

The video also discusses how usage limits can interrupt long-running work.

An interrupted agent may leave a task partially complete, requiring the developer to recover the work and restore the necessary context.

A more resilient workflow should preserve:
- Current task status.
- Completed steps.
- Remaining work.
- Intermediate artifacts.
- Verification results.
- Known errors and recovery instructions.

For example:

```text
Task Started
     |
     v
Execute Stage 1
     |
     v
Save Intermediate State
     |
     v
Execute Stage 2
     |
     +---- Interrupted ----> Save Status and Resume Later
     |
     v
Run Verification
     |
     v
Report Final Result
```

This is a general resilience pattern rather than a claim that every coding-agent product automatically provides complete checkpoint recovery.

**Practical lesson:** Long-running agents need explicit recovery strategies, not just instructions to keep working.

---

# Core Takeaways

1. **Test before adding complexity.** Determine what the model can already do before building an extensive skill or workflow around it.

2. **Skills should address demonstrated weaknesses.** Preserve instructions that improve performance and remove unnecessary ones.

3. **Planning is a human oversight tool.** A capable model may plan internally, but a visible plan remains useful for complex or risky changes.

4. **Optimize the entire workflow.** Consider quality, retries, execution time, and verification—not just individual model-call costs.

5. **Delegate with clear boundaries.** Subagents and forked conversations serve different context and execution needs.

6. **Use structured workflows for complex tasks.** Multi-agent patterns can help, but unnecessary agents add cost and coordination overhead.

7. **Make completion verifiable.** Define explicit conditions and require appropriate evidence before declaring a task finished.

8. **Combine probabilistic reasoning with deterministic checks.** Model-based judgment and executable tests serve different purposes.

9. **Design for interruptions.** Persist intermediate results and provide a clear recovery path for long-running work.

10. **Reassess workflows as models improve.** Techniques that were necessary previously may become redundant, while verification and security requirements can remain essential.

---

# Application to My Agentic WMS

## 1. Reconsider the Role of Each Agent

For an AI-agent-based Warehouse Management System, each agent should have a clearly defined responsibility and a measurable contribution.

Potential examples:

| Agent | Responsibility | Verification |
|---|---|---|
| Warehouse Advisor Agent | Answer operational questions using warehouse data | Validate retrieved records and cite their source |
| Discrepancy Agent | Investigate differences between expected and observed inventory | Compare records, observations, and reconciliation results |
| Space Utilization Agent | Recommend storage and slotting improvements | Validate capacity, compatibility, and location constraints |

The goal is not to create an agent for every feature.

An agent is justified when its specialized responsibility, interface, or execution process improves the overall system.

## 2. Treat Skills as Warehouse-Specific Knowledge and Corrections

A warehouse agent may need specialized knowledge about:
- Inventory terminology.
- Location hierarchies.
- Storage constraints.
- Replenishment rules.
- Company-specific operational procedures.
- Escalation requirements.

Not all of this knowledge belongs in a skill.

Some information should live in structured data or a knowledge base. Other information may be expressed as reusable agent instructions.

A useful separation is:

```text
Warehouse Data
      |
      v
Structured Queries / APIs
      |
      v
Agent Reasoning
      |
      v
Domain-Specific Instructions
      |
      v
Validated Recommendation
```

The agent should retrieve changing inventory facts from authoritative sources rather than relying on instructions or conversational memory to maintain them.

## 3. Build Verification into the Discrepancy Workflow

Suppose a drone observes an item count that differs from the WMS record.

A naive agent might immediately conclude that inventory is incorrect.

A better workflow would be:

1. Retrieve the expected stock record.
2. Obtain the drone's observation and its associated metadata.
3. Check that the observation corresponds to the correct warehouse location.
4. Assess whether the observation is sufficiently reliable.
5. Compare the expected and observed quantities.
6. Record the discrepancy and supporting evidence.
7. Request human review when the discrepancy cannot be resolved confidently.

The key principle is to separate **observation, inference, validation, and action**.

A model-generated conclusion should not automatically become a change to the authoritative inventory record.

## 4. Use Explicit Definitions of Done

Each WMS agent should have its own completion criteria.

For example:

**Discrepancy investigation**
- The expected inventory record has been retrieved.
- The observation has been associated with a specific location.
- The quantity comparison has been calculated.
- Relevant evidence has been recorded.
- Unresolved uncertainty has been reported.
- Any required escalation has been triggered.

These conditions make agent behavior easier to evaluate and audit.

## 5. Separate Software Agents from Drone Control

The concepts in this video are useful for orchestrating software agents, but they do not eliminate the need for conventional robotics engineering.

A sensible architecture would separate:

- **Reasoning layer:** Interprets warehouse questions and proposes actions.
- **Orchestration layer:** Routes tasks between specialized agents.
- **WMS integration layer:** Retrieves and updates inventory information.
- **Perception layer:** Processes drone imagery and sensor observations.
- **Robotics control layer:** Handles navigation, motion, and safety constraints.
- **Verification layer:** Validates proposed changes and task completion.

LLM-based agents can help plan and coordinate warehouse operations, but time-critical control and safety-critical constraints should be enforced by appropriate robotics and control systems.

## 6. Evaluate Whether Each Agent Is Necessary

For a prototype, compare different implementations of the same task.

For example:

| Approach | Purpose |
|---|---|
| Direct API query | Establish a non-agentic baseline |
| Single agent with structured tools | Measure the benefit of model-based interpretation |
| Agent with domain-specific instructions | Measure the benefit of additional instructions |
| Multi-agent workflow | Determine whether task decomposition improves results |

Measure:
- Accuracy.
- Latency.
- Cost.
- Failure rate.
- Verification success.
- Human intervention required.

This prevents unnecessary architectural complexity and helps identify where agentic reasoning creates genuine value.

---

# Broader Architectural Lessons

## Agent Capability Is Not the Same as System Reliability

A model may be capable of producing a correct result without consistently producing one.

Production systems must account for both capability and reliability.

## Context Should Be Deliberate

Not every agent needs the entire conversation, every document, or every intermediate result.

Pass the information needed to perform the assigned task, and preserve additional context when the task genuinely depends on it.

## Verification Should Be Independent Where Necessary

If the same model generates an output and then simply asserts that it is correct, the second assertion is not independent evidence.

Stronger verification may require tests, authoritative data, a separate review process, or human approval.

## Agentic Systems Need Operational Controls

As agents gain more autonomy, the system must manage permissions, execution limits, recovery, and the consequences of mistakes.

These requirements cannot be solved by prompt engineering alone.

---

# Questions for Further Exploration

- How should agent skills be evaluated automatically?
- Which parts of a warehouse workflow benefit from LLM reasoning rather than deterministic code?
- How should context be passed between agents without losing essential information?
- How can an agent prove that a task is complete?
- Which tasks require independent verification?
- How should an orchestrator recover after an agent is interrupted?
- When is a multi-agent workflow justified over a single agent?
- How can agent permissions be restricted to the minimum necessary capabilities?
- How should a guardrails system validate proposed actions before they reach warehouse APIs or robotics interfaces?

---

# Related Obsidian Notes

- [[AI Agents]]
- [[Agentic WMS Architecture]]
- [[Multi-Agent Systems]]
- [[Agent Orchestration]]
- [[Context Engineering]]
- [[LLM Agent Harnesses]]
- [[Agent Skills]]
- [[Agent Security and Guardrails]]
- [[Model Context Protocol (MCP)]]
- [[Agent Evaluation]]
- [[Software Testing and Verification]]
- [[Autonomous Warehouse Robotics]]
- [[Digital Twins]]

---

# Source

**Video:** [Claude Changed Completely — Every NEW Concept Explained](https://www.youtube.com/watch?v=YAuWBqZ_gjs)

**Speaker:** Simon Scrapes

**Source note:** This summary is based on the available transcript-derived material and its reported demonstrations. Product capabilities, commands, and experimental features may change. The architectural recommendations for warehouse management are applications of the video's concepts, not demonstrations from the original video.
