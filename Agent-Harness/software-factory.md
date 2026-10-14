---
tags: [AI, Software-Factory, Harness-Engineering, Agents, Tessl, AI-Engineer]
source: https://youtu.be/X6l4lpA0_NY?si=4ucbFhc3S2uRv8RX
channel: AI Engineer
speaker: Dru Knox
published: October 2026
---

# Harness Engineering: How to Build a Software Factory

## Overview: The Core Thesis
In this talk, Dru Knox (Head of Product & Design at Tessl) challenges the common assumption that AI coding agents force a tradeoff between shipping speed and software quality (e.g., getting more features but accepting "AI slop"). He argues this is a short-term illusion. 

The ultimate payoff of a functioning "AI Software Factory" isn't just speed—it's **superior quality**. When a software factory works correctly, the standard engineering backlog disappears, freeing up human developers to focus entirely on architectural refactoring, test quality, and system design rather than routine feature work.

## Key Concepts & Challenges

### 1. Autonomy vs. Automation (The Sequence of Success)
Knox stresses that organizations fail when they try to jump straight to full automation. Teams must build metrics and capabilities in a strict sequence:
1. **Autonomy:** Can the agent one-shot a problem? (Even if a human still has to review every line of code, the agent is highly autonomous).
2. **Automation:** Can we remove the human from the manual review process? (This is only possible once autonomy is reliable).
3. **Quality:** How good is the end product for the user? (Measured by user analytics, test coverage, etc.).

### 2. The Control Plane
Before anything else, you need a control plane to make all work *legible*. Most data needed to improve agents is inaccessible. You must move workflows onto surfaces where every touchpoint is recorded (e.g., Issue Tracker -> Headless Agent in Sandbox -> Pull Request -> Engineer Comments). Without this traceability, agents can never improve.

## The 3 Loops of Harness Engineering
Harness engineering is the practice of building the "production cage" and feedback loops that allow agents to safely operate and improve. It maps to three distinct phases of development:

### 1. The Inner Loop
- **When it runs:** While the coding agent is actively working on a Pull Request (before submitting it).
- **Purpose:** Fast, cheap corrections. It catches and corrects the agent's mistakes *without* human intervention, driving autonomy.

### 2. The Outer Loop
- **When it runs:** Once the PR is officially put up.
- **Purpose:** Expensive, comprehensive checks (like heavy CI/CD testing, security scanning) to ensure the code is safe to merge.

### 3. The Meta Loop
- **When it runs:** Continuously across the system.
- **Purpose:** Observes logs, PRs, issues, and user feedback. This is where engineers spend most of their time, figuring out *why* the factory failed on a task and updating the playbooks/skills to prevent that failure in the future.

## Tessl's Approach
Knox concludes by outlining Tessl's role as a "batteries-included" platform for harness engineering. Tools like *Tessl Launch* allow teams to turn any skill into an automated workflow, letting agents (like Codex, Claude Code, or Gemini) run in secure sandboxes with appropriate permissions, streamlining the transition from standalone coding agents to a true software factory.

---
*Note: This summary pairs excellently with notes on Agentic Workflows, CI/CD evolution, and AI-driven testing strategies.*
```eof

The file is complete! You can copy and paste this directly into your Obsidian vault. Let me know if you need summaries for any other talks or tutorials!
http://googleusercontent.com/youtube_content/1
http://googleusercontent.com/youtube_content/2
