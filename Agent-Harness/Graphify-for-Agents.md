# Graphify for Agents

Source: [How to Use the Graphify Skill to Build a Queryable Knowledge Graph for AI Agents](https://www.mindstudio.ai/blog/how-to-use-graphify-skill-knowledge-graph-ai-agents)

## Summary

Graphify turns codebases, notes, and documentation into a queryable knowledge graph for AI agents. Instead of feeding raw files into the model every time, the agent can query structured relationships between entities.

This helps agents work with large projects because they can retrieve only the relevant subgraph instead of loading entire repositories or note folders into context.

## Core Idea

A knowledge graph stores information as:

- **Nodes**: entities such as files, classes, functions, concepts, APIs, documents, or terms.
- **Edges**: relationships such as calls, imports, depends on, references, checks, belongs to, or relates to.

For agents, this is useful because graph queries preserve structure. Vector search is good for semantic similarity, but graph search is better for questions like:

- What depends on this function?
- Which service calls this API?
- Which notes reference this concept?
- What breaks if this module changes?

## What Graphify Does

Graphify takes source material and:

1. **Parses content**: extracts entities from code, notes, or documents.
2. **Infers relationships**: maps calls, dependencies, references, and conceptual links.
3. **Builds a graph**: stores entities as nodes and relationships as labeled edges.
4. **Exposes queries**: lets agents ask natural language questions or perform structured traversals.

The output is not just a summary. It is a reusable graph the agent can query repeatedly.

## Best Use Cases

- **Codebase Q&A**: answer architecture questions without reading every file.
- **Repository onboarding**: help a developer or agent understand an unfamiliar project.
- **Refactoring impact analysis**: find what depends on a function, class, module, or service.
- **Documentation agents**: answer questions about API structure, authentication, shared parameters, or system design.
- **Research notes**: map relationships between concepts, papers, ideas, and references.

## Setup Requirements

The article assumes:

- Node.js 18 or later.
- MindStudio Agent Skills SDK.
- MindStudio API key.
- Source material such as a codebase, markdown notes, or documentation set.
- Basic async JavaScript or TypeScript knowledge.

Important guideline:

- Store API keys in environment variables. Do not hardcode them.

## Preparing Source Material

Graph quality depends heavily on input quality.

For codebases:

- Include the full directory path, not isolated random files.
- Keep file extensions consistent.
- Exclude generated files, build artifacts, vendored code, lock files, and `node_modules`.
- Include README files or architecture notes when available.

For notes and documentation:

- Use consistent markdown heading structure.
- Give files descriptive names.
- Group related notes into directories.
- Keep code and prose separated when possible.

For mixed inputs:

- Separate code and documentation into distinct folders.
- Mark the content type explicitly when calling Graphify.

## Query Patterns

Graphify supports three useful query styles.

### Natural Language Query

Use plain English when exploring:

- "What does this module depend on?"
- "What functions call `validateToken`?"
- "Which notes mention retrieval-augmented generation?"

This is good when the agent does not yet know the exact graph structure.

### Structured Traversal

Use traversal when the agent knows the starting entity and relationship type.

Useful parameters:

- Start node.
- Node type.
- Relationship type.
- Traversal depth.

This is better for precise dependency checks and refactoring analysis.

### Subgraph Extraction

Use subgraphs to give an LLM focused context around one topic or entity.

Instead of passing thousands of raw tokens from source files, extract a small connected cluster of relevant nodes and relationships.

## Agent Loop Pattern

The article recommends embedding Graphify into the agent loop rather than treating it as a one-time lookup.

Use this pattern:

1. **Identify**: ask the graph which entities are relevant to the user question.
2. **Expand**: traverse relationships around the most relevant entities.
3. **Answer**: pass only the focused graph context into the LLM.

This keeps each model call smaller and more relevant.

## Keeping Graphs Updated

Knowledge graphs become stale when the source changes.

Guidelines:

- Use incremental updates when only a few files changed.
- Use full rebuilds after major refactors or large module additions.
- Connect updates to CI or a file watcher for active codebases.
- Maintain separate graph IDs for dev, staging, and production.

## Common Mistakes

- **Including generated or vendored code**: bloats the graph with low-value nodes.
- **Querying too broadly**: returns too many nodes and reintroduces context bloat.
- **Rebuilding too often**: wastes time and compute when incremental updates would work.
- **Skipping metadata**: removes file paths and line numbers, making answers harder to verify.
- **Mixing environments**: using one graph for dev, staging, and production causes confusing results.

## Practical Guidelines

- Use Graphify when structure matters more than similarity search.
- Keep metadata enabled so agents can cite file paths and line numbers.
- Limit broad queries with `maxNodes` or follow-up traversal.
- Partition very large codebases by module or service.
- Let multiple agents share the same graph ID when they need the same project context.
- Prefer graph traversal for dependency, ownership, and blast-radius questions.
- Prefer vector search for fuzzy semantic recall.
- Combine graph queries with LLM reasoning, not as a replacement for reasoning.

## Practical Takeaway

Graphify is useful when agents need to understand large, connected bodies of information without loading everything into context. The strongest pattern is to build a graph once, keep it updated incrementally, and let agents query relevant entities and relationships before answering or modifying anything.
