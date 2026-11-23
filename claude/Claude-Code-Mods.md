---
tags: [AI, Claude-Code, Mods, Plugins, Hooks, Prompt-Caching, Context-Engineering, Productivity]
source: https://www.youtube.com/watch?v=bTqizdva04g
channel: Ben AI
speaker: Ben (Ben AI)
title: "9 Insane Claude Mods I Already Can't Live Without"
length: ~14 min
---

# 9 Claude Code Mods Worth Using

## Overview
Anthropic shipped **mods** for Claude Code. They let Claude build features *for itself*: you describe a feature you wish Claude Code had, and Claude builds it into the app. Ben tried more than 100 mods and picked 9 favorites. Several he built himself, and the rest were inspired by community mods or came from Anthropic.

## What Is a Mod?
- A **plugin that changes how Claude Code looks and behaves**, in both the desktop app and the terminal. It customizes the interface and the harness itself.
- **How it works:** Claude Code emits a *signal* (event) whenever something happens: a prompt is sent, a tool is about to run, a subagent starts, a reply finishes. A mod **listens** for one signal and can then **watch, change, or block** what happens. With those building blocks you can build almost any feature.
- **How you build one:** describe it in plain language. Claude uses its built-in **`plugin-authoring`** skill to write the mod. To adjust it, tell Claude what to change, the same way you would with a skill.
- **Two kinds of mods:**
  1. **AI-powered:** they call a model, so they use your plan's usage like a normal message.
  2. **Display/tracking only:** they draw or track things on screen and **cost nothing** to run.

> [!tip] Why it matters
> Ben argues that mods are a reason to move from Cowork to the **Claude Code** tab. Mods can fill in the UI features that Code lacked.

## The 9 Mods

| # | Mod | Hooks into | Uses AI? | What it does |
|---|-----|-----------|----------|--------------|
| 1 | **Brain Dump** | Prompt submitted | Yes | Turns messy voice brain dumps into a structured prompt |
| 2 | **Helper Desk** | Subagent start, to-do updates | No | Progress bar and a per-subagent cost/progress panel |
| 3 | **Cache Clock** | Reply finished | Only for keep-warm pings | Countdown until the prompt cache goes cold; optional keep-warm |
| 4 | **Handoff** | 250k tokens, cold cache, or `/handoff` | Yes | Writes a detailed handoff prompt and opens a fresh chat |
| 5 | **Reflect** | Prompt submitted | Yes | Spots "hard rules" you give and offers to save them to memory |
| 6 | **Chat Inventory** | Every tool, connector, and skill call | No | Cowork-style "In this chat" side panel |
| 7 | **Screen Share Mode** | `/screenshare` toggle | No | Grays out names, emails, phone numbers, and dollar amounts |
| 8 | **Replay Theater** | File edits, `/replay` | No | Plays back each file edit; lets you roll back |
| 9 | **Switchboard** | `/mods` | No | Lists every running mod with on/off toggles |

### 1. Brain Dump
- **Problem:** long voice brain dumps are messy. They contain transcription errors, jump around, and sometimes contradict themselves.
- **How it works:** listens for prompt submission. If the prompt looks like a long brain dump, it **blocks** it, asks a few **clarifying questions** using a background model, and rewrites it into Anthropic's prompting framework:
  - the **task**, the **why**, the **guardrails**, and **what "done" looks like**.
- Claude then works from the clean prompt instead of the raw dump.
- Example build prompt: *"Build me a mod that catches my long voice brain dumps, asks me a few clarifying questions, and then rewrites the prompt into my prompting framework."*

### 2. Helper Desk
- **Problem:** newer models such as Opus 5.5 start many subagents, for research for example, and you can't see what each one is doing, what it costs, or how far along it is.
- **What it shows:**
  - A **purple progress bar** above the prompt box that fills as to-dos get ticked off, with the current step.
  - A **side panel** with one colored "Claude crab" per subagent, showing its own progress bar and **cost**, plus how they add up to the chat's total.
- Display only, so it costs nothing. You can ask Claude to restyle it, for example to name each subagent after its role.
- Inspired by Johnny Savy's progress mod.

### 3. Cache Clock
- **Background: prompt caching.**
  - Each message makes Claude reread the whole chat. Anthropic caches the chat after each reply ("keeping it warm"). Warm reads cost a fraction of the normal price, about **1/20th on Opus 5.5**.
  - The cache **expires**: **1 hour on a Claude subscription** and **5 minutes with an API key**. After that the chat goes **cold** and the full chat is reprocessed at full price.
  - On a long chat this can make the same message **up to ~40× more expensive**. Ben's Opus 5.5 example is **~$0.04 warm vs. ~$1.60 cold** each time you return.
- **What the mod does:** after each reply it shows a **countdown until the cache goes cold**. The timer comes from Anthropic's own response, not a guess.
- **Keep warm:** click *Keep warm* or type `/keepwarm`. Just before expiry it sends a small hidden message that resets the timer, costing a few cents instead of a cold restart. It **stops automatically** when you send your next message or after **2 hours**.
- Inspired by Karan Binsal's "cache tax" mod.

### 4. Handoff
- **Problem:** long chats degrade. The context window fills up and you get **context rot** (missed details, mixed-up facts). **Compaction** summarizes older turns but loses detail, and a cold cache makes each message expensive on top of that.
- **Triggers:**
  1. The chat reaches **~250,000 tokens**, which Ben treats as roughly where performance starts to slip.
  2. Cache Clock detects that the cache **went cold**.
  3. You type **`/handoff`**.
- **What it does:** reads the whole chat plus every file, skill, tool, and link used, and writes a **structured handoff prompt** that is much more detailed than a compaction summary. You get a button to **start a fresh chat** with that prompt, covering context, progress, and next steps.
- **Bonus:** since it's just a prompt, you can also hand the task to a teammate.
- Works well together with Cache Clock.

### 5. Reflect
- **Problem:** auto-memory, `CLAUDE.md`, `AGENTS.md`, and "second brain" setups exist, but you don't control what gets saved, and important rules are often forgotten.
- **What it does:** checks every prompt for a **hard rule** (for example *"never use em dashes"* or *"keep LinkedIn hooks under 12 words"*). When it finds one, a pop-up asks where to save it: **built-in memory**, **CLAUDE.md**, or your **second brain**.
- You can ask Claude to change where rules are saved, for example into a specific skill.
- Inspired by a "Claude reflect" skill.

### 6. Chat Inventory
- **Problem:** the Code tab feels like a **black box** on long tasks. Cowork has a side panel showing to-dos, connectors, skills, and files.
- **What it does:** tracks every tool call, connector call, and skill use in an **"In this chat"** side panel showing:
  - the to-do list with the **goal** at the top
  - each **connector** with its logo and call count
  - **skills** used
  - **files** read and created
- Useful for auditing where results came from, for example a client report built from the CRM, email, and Drive.
- Tracking only, so it costs nothing. This is the mod that got Ben to switch from Cowork to Code.

### 7. Screen Share Mode
- **Problem:** screen shares, sales calls, trainings, Looms, and recorded videos can leak client names, emails, deal values, and personal details.
- **What it does:** type **`/screenshare`** to turn it on. Every **name, email, phone number, and dollar amount** shows as a **gray bar**.
- Lets Ben record real chats instead of making up fake client data.

### 8. Replay Theater *(from Anthropic)*
- **Problem:** in copywriting or design work, Claude can make a dozen edits at once and you can't tell what changed.
- **What it does:** records every change Claude makes to a file. After a reply, type **`/replay`** to play the edits back **one by one** and **revert to an earlier version** if you prefer it.
- **Limitation:** works only on **files**, not text in the chat. Your draft must be a document that Claude edits.

### 9. Switchboard
- **Problem:** mods will pile up the way skills did, and there's no quick way in the desktop app to see what's running or turn one off mid-chat.
- **What it does:** **`/mods`** opens a switchboard listing every running mod. You can **toggle each on or off** and **hide the extra panels** some mods add. New mods appear in the list automatically.

## Key Takeaways
- **Mods = event hooks + UI.** Listen for a signal, then watch, change, or block. That is enough to build most features you'd want.
- **Build by describing.** The `plugin-authoring` skill writes the mod, and you change it by asking Claude.
- **Cost awareness:** display-only mods are free. AI-powered mods use plan usage.
- **Biggest practical wins:**
  - **Token cost:** Cache Clock plus Handoff target cold-cache costs and context rot.
  - **Visibility:** Helper Desk and Chat Inventory make subagents and tool use visible.
  - **Memory hygiene:** Reflect makes rule capture explicit.
  - **Mod sprawl:** Switchboard keeps installed mods manageable.

## Related
- [[claude-code-system]]
- [[Harness-engineering]]
- [[memory-aware-agents]]

> [!note] Source note
> Summary written from the video's auto-generated English transcript. Ben says all 9 mods are free in a zip file linked in the video description; you can drop the zip into a Claude chat and ask Claude to install them. The sponsor segment for Ben's "AI Accelerator" is left out.
