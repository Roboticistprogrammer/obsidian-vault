---
title: "How One Founder Manages 20 AI Coding Agents at Once"
speaker: "Ryan Carson"
organization: "Untangle"
source: "https://www.youtube.com/watch?v=BTtyguoPvBU"
video_id: "BTtyguoPvBU"
duration: "15:28"
type: "video-summary"
topics:
  - AI coding agents
  - agent orchestration
  - cloud development
  - software engineering automation
  - QA and code review
summary_status: "public-description-based"
---

# How One Founder Manages 20 AI Coding Agents at Once

> [!warning] Source limitation
> This note is based on the publicly indexed talk description and event listing, not a verified full transcript. The broad architecture and practices below are supported by that description; exact implementation details, tools, and examples should be checked against the video. No unverified timestamps are attributed to individual claims.

## Overview

Ryan Carson describes a software-development workflow in which a human works primarily through an **engineering-manager agent**. That agent coordinates a group of cloud-based builder agents—up to 20 at a time—through a ticket board.

Rather than manually prompting and supervising every coding agent, the system introduces a management layer and automated control loops:

- An engineering-manager agent assigns and coordinates work.
- Builder agents implement individual tickets in cloud environments.
- An automation checks on builders every 15 minutes.
- Review loops are capped to prevent agents from cycling indefinitely.
- Dedicated QA agents test completed work.
- A merge-queue manager coordinates integration.
- The human reviews a video of each feature before it ships.

The important shift is from **AI as an individual coding assistant** to **AI as a managed software delivery workforce**.

## 1. The orchestration model

```text
                  Human founder / operator
                           |
                    Voice interaction
                           |
                 Engineering-manager agent
                           |
                     Ticket board
          _______________|________________
         |               |                |
     Builder 1       Builder 2         Builder N
         |               |                |
     Cloud work      Cloud work       Cloud work
         |               |                |
         +---------------+----------------+
                         |
                Automated progress checks
                    (every 15 minutes)
                         |
                 Bounded review loops
                         |
                     QA agents
                         |
                Merge-queue manager
                         |
                 Human feature review
                         |
                       Release
```

The engineering-manager agent acts as the coordination layer. The builder agents are execution workers. This separation allows the human to focus on priorities and acceptance rather than micromanaging every implementation step.

## 2. Engineering-manager agent

The manager agent is the main interface between the human and the agent workforce.

Its responsibilities, as described in the public talk summary, include:

- Receiving work requests from the human, often through voice.
- Translating requests into actionable tickets.
- Coordinating builder agents against the ticket board.
- Monitoring progress and identifying stalled work.
- Managing the flow of work through review and integration.

This resembles a human engineering manager's role: decomposing work, assigning ownership, tracking progress, and coordinating delivery.

**Key design principle:** avoid making the human the message router between every worker agent.

## 3. Parallel builder agents

Builder agents work on separate tickets in cloud environments. The system can manage up to 20 simultaneously.

Parallelism can increase throughput, but it also creates coordination risks:

- Multiple agents may modify overlapping parts of the codebase.
- Agents may misunderstand ticket boundaries.
- Work may finish at different times.
- Integration conflicts can accumulate.
- More agents can create more review and QA work, not just more code.

A ticket board provides a shared representation of what is requested, assigned, in progress, and ready for review.

> [!tip] Practical implication
> Parallelize work only when tasks have clear boundaries and acceptance criteria. Splitting one tightly coupled feature across many agents can increase integration cost rather than reduce it.

## 4. Automated monitoring and bounded review

The talk description highlights an automation that checks every builder every 15 minutes.

This creates a recurring supervision loop:

1. Inspect the builder's current state.
2. Determine whether it is progressing, blocked, or finished.
3. Request an update or route the work to the next stage.
4. Avoid endless review cycles by enforcing a maximum number of iterations.

The review-loop cap is especially important. Without a limit, agents can repeatedly revise code or respond to reviewer feedback without converging.

A robust implementation should define explicit exit conditions, such as:

- Tests pass.
- Required files or outputs exist.
- Acceptance criteria are satisfied.
- No blocking review findings remain.
- The task is escalated to a human when the retry limit is reached.

## 5. QA agents and merge-queue management

The workflow separates implementation from quality assurance and integration.

- **Builder agents** create or modify code.
- **QA agents** check whether the feature behaves as expected.
- **Merge-queue manager** coordinates when completed changes can be integrated.

This separation is useful because an agent that wrote a change should not be the only mechanism deciding whether that change is correct.

A merge queue also helps manage concurrency. It can serialize or control integration so that independently completed tasks do not destabilize the shared codebase.

## 6. The human review that remains essential

One notable practice in the talk description is that Ryan watches a video of each feature before it ships.

This is a human acceptance gate. Automated tests can verify many properties, but a short recording can reveal user-facing problems that tests may miss:

- Confusing interaction flows.
- Visual defects.
- Unexpected behavior.
- Missing states or incomplete workflows.
- A feature that technically works but does not meet the intended experience.

The broader lesson is that **delegating implementation does not mean delegating accountability**. The human still owns the product decision to ship.

## 7. What makes this architecture work?

| Mechanism | Purpose | Failure it helps prevent |
|---|---|---|
| Engineering-manager agent | Coordinates work and workers | Human becoming the bottleneck |
| Ticket board | Makes tasks and status visible | Unstructured parallel work |
| Cloud builder agents | Execute tasks concurrently | Serial, one-agent-at-a-time development |
| 15-minute checks | Detect stalled or completed work | Workers running without supervision |
| Capped review loops | Bound retries and revisions | Infinite agent-review cycles |
| QA agents | Independently test implementation | Self-approval by the coding agent |
| Merge-queue manager | Control integration | Conflicting or unsafe merges |
| Human feature-video review | Validate the actual experience | Shipping technically passing but poor features |

## 8. Applying the ideas to Aerostack WMS

The same pattern could support development of Aerostack's WMS platform, especially as its agentic workflows and robotics integrations grow.

### Example: building a warehouse discrepancy workflow

```text
Human product owner
       |
WMS engineering-manager agent
       |
       +--> Backend builder
       |      - discrepancy APIs
       |      - database changes
       |
       +--> Frontend builder
       |      - discrepancy dashboard
       |      - investigation UI
       |
       +--> Integration builder
       |      - WMS event integration
       |      - drone observation adapter
       |
       +--> Test/QA agent
       |      - API tests
       |      - workflow tests
       |      - edge cases
       |
       +--> Documentation agent
              - API docs
              - operator guide
       |
       v
  Integration and merge queue
       |
  Human acceptance review
       |
  Release
```

### Potential engineering-manager responsibilities

For Aerostack, the manager agent could:

- Convert product requirements into implementation tickets.
- Identify dependencies between database, API, UI, and simulation work.
- Assign independent tasks to separate builders.
- Require tests and evidence before moving tickets to review.
- Escalate architectural decisions rather than allowing agents to invent incompatible designs.
- Keep a record of decisions and implementation status.

### Important safeguards for a robotics-connected product

A coding-agent workflow should be kept separate from operational control of physical drones. Agents developing software should not automatically gain authority to issue real drone commands.

Recommended boundaries:

1. Use simulated environments for development and testing.
2. Require explicit authorization for deployment to hardware-connected systems.
3. Validate commands and safety constraints outside the language model.
4. Keep audit logs for code changes, deployments, and operational actions.
5. Require human approval for safety-critical changes.
6. Test failure modes, including lost connectivity, stale inventory data, and conflicting observations.

## 9. Key takeaways

1. **Think in teams, not single agents.** A manager agent can coordinate multiple specialized coding agents.
2. **Use tickets as the shared contract.** Clear task boundaries and acceptance criteria are essential for parallel execution.
3. **Automate supervision.** Periodic checks help identify stalled or completed work.
4. **Bound agent loops.** Retry and review limits prevent endless cycles.
5. **Separate building from verification.** QA and merge management should be explicit stages.
6. **Keep humans accountable for shipping.** A human review of the real feature remains a valuable final gate.
7. **More agents do not automatically mean more throughput.** Coordination, integration, and review capacity must scale too.

## 10. Questions to answer while watching

- Which cloud coding-agent platform and model does Untangle use?
- How does the engineering-manager agent choose and assign tickets?
- What information does the 15-minute automation inspect?
- How are builder agents isolated from one another?
- What exactly counts as a completed ticket?
- How many review attempts are allowed, and what happens after the limit?
- What checks do QA agents run?
- How does the merge-queue manager resolve conflicts?
- How are feature videos recorded and evaluated?
- What are the cost and latency trade-offs of running up to 20 agents?

## Related Obsidian notes

- [[Agent Harnesses]]
- [[Multi-Agent Orchestration]]
- [[AI Coding Agents]]
- [[Agent Evaluation and Verification]]
- [[Human-in-the-Loop Systems]]
- [[MCP and A2A]]
- [[Aerostack WMS Architecture]]

## Source

- [YouTube: How One Founder Manages 20 AI Coding Agents at Once — Ryan Carson, Untangle](https://www.youtube.com/watch?v=BTtyguoPvBU)
- Public talk listing and synopsis: AI Engineer / NileCast, November 10, 2026.
