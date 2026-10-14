---
tags: [AI, Data-Engineering, Ontologies, Architecture, Jev, Agents, Southbridge-AI, AI-Engineer]
source: https://youtu.be/Fa3hQVIKShw?si=fguAam9H5yKRmg4K
channel: AI Engineer
speaker: Hrishi Olickel
published: October 2026
---

# Just-in-Time Ontologies: What Cheap Intelligence Means for Data

## Overview: The Core Thesis
In this talk, Hrishi Olickel (founder of Southbridge AI) argues that the fundamental bottleneck in data transformation—general intelligence/judgment—has been solved and made cheap. Historically, data infrastructure was built on the assumption that transforming unstructured data into structured data was expensive. 

This led to the "universal schema" or "enterprise ontology" fallacy—the idea that you can map all corporate data into a single master structure ahead of time. Olickel argues that **single ontologies always fail** because data schema needs are entirely dependent on the *decision* being made at the moment of query. The solution is **Just-in-Time Ontologies**: leveraging cheap AI models (like Jev) to transform and structure data dynamically only when the decision space is known.

## Key Concepts & Challenges

### 1. The Four Schemas
You can never map data into a single schema because information is naturally constrained by four different schemas:
1. **Source Schema:** The constraints of the device or person recording the data (e.g., a phone vs. a desk sensor).
2. **Storage Schema:** The constraints of the database/system storing the data (e.g., unable to store firmware details or microsecond timestamps).
3. **Decision Schema:** The specific context of the question being asked. (You only know what "customer" means when you know if you are billing them, supporting them, or checking compliance).
4. **Prior Schema:** The inherent biases and beliefs that the AI model brings to the data.

### 2. The Three Roles of Models in Data Pipelines
A common mistake in agentic architecture is mashing different intelligence tasks into one context window. Models should be split by role:
1. **The Builder/Designer:** Needs slower, highly capable, "heavy token" reasoning models (like Claude 3.5 Sonnet/Opus) to read docs, write specs, and design the pipeline.
2. **The Worker:** Lives *inside* the pipeline. Needs cheap, fast, hyper-specialized models (like Jev) that follow instructions flawlessly and do the actual data crunching.
3. **The Hill Climber (Surfer):** Models that self-optimize the pipeline by running experiments, adjusting budgets, and finding the most optimal configuration.

### 3. Model Priors & Intersection
Just like humans, LLMs have built-in "priors" (assumptions) that can corrupt data. 
- *Example:* A model might see negative numbers in a loan dataset, assume the negative sign is redundant, take the absolute value, and accidentally turn $784,000 of cash into $5.3 million of debt.
- *Solution:* **Cross-Model Prior Intersection.** By having two models from completely different pedigrees/families check the same data, you can drastically reduce errors. If they agree, it's almost certainly right; if they disagree, a human or a reasoning model steps in.

## Methodologies

### Burrito Surfing
"Burrito Surfing" is Olickel's term for an agentic process of pipeline self-optimization. It requires:
1. **A stable deployment surface** (e.g., Southbridge's open-source *Hankwave* harness).
2. **Bounded experiments** (cost, time, and access limits).
3. **A defined failure surface** (a baseline to measure success against).
Once set up, the system runs minimal bounded pilots. It finds an architecture that works, sets it as a baseline, and then systematically swaps out models for cheaper/faster options, tightening the requirements each time until the pipeline is completely optimized for cost, speed, and accuracy.

### Case Study: Ohio Campaign Finance
Using the Jev model, Southbridge built an entity resolution pipeline for Ohio's campaign finance records.
- **Original projection:** $10,000.
- **Actual cost via "Burrito Surfing" agents:** $7.95 (to process 23,000+ cases).
- The agent built the pipeline, tested over a dozen architectures, compared models, and found the cheapest reliable path in just 10 days.

---
*Note: This summary links well with concepts regarding data modeling, agentic design patterns, and the economics of LLM API calls.*
```eof

The summary is ready for your Obsidian vault. Let me know if you need anything else!
http://googleusercontent.com/youtube_content/1
