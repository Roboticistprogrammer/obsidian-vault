# Spec-Driven Development

Source: [Spec-Driven Development in 2026: What It Is, the Tooling, and How Teams Actually Use It](https://dev.to/krlz/spec-driven-development-in-2026-what-it-is-the-tooling-and-how-teams-actually-use-it-2fk2)

## Summary

Spec-Driven Development (SDD) treats the specification as the main artifact of software development, especially when AI agents write much of the code. Instead of relying on loose prompts or design docs that go stale, SDD asks teams to define intent clearly, turn that intent into executable checks, and use code as the implementation that must satisfy the spec.

The article frames SDD as a disciplined response to "vibe coding": AI can generate code quickly, but it needs precise requirements, review gates, and validation to avoid guessing wrong. The human role shifts from typing most implementation details to clarifying intent, reviewing plans, checking tasks, and maintaining traceability between requirements, tests, and code.

## Important Points

- **SDD is not just better documentation.** The key difference is that specs should be executable or continuously validated through tests, contracts, BDD scenarios, simulations, or CI checks.
- **The practical target is spec-anchored development.** Specs and code evolve together, while tests enforce alignment. This is more realistic for most teams than fully generating all code from specs.
- **Spec-as-source is still mostly aspirational.** Editing only specs while never hand-editing generated code requires mature tooling and high trust in generation.
- **Do not jump directly from spec to code.** The article emphasizes a staged workflow: constitution, specify, clarify, plan, tasks, implement, analyze.
- **Acceptance criteria need precision.** EARS-style requirements help make specs less ambiguous and easier to convert into tests.
- **Traceability is the main value.** Tasks, tests, and commits should point back to specific spec clauses so teams can see which intent each implementation satisfies.
- **Tooling helps, but does not replace thinking.** Spec Kit, Kiro, Claude Code workflows, Cursor Plan Mode, OpenSpec, BMAD-METHOD, Tessl, and similar tools are useful only if the team keeps the spec disciplined.
- **SDD is worth using when complexity and maintenance matter.** It fits AI-assisted production work, complex requirements, multiple maintainers, integration-heavy systems, and regulated domains.
- **SDD is probably overkill for throwaway prototypes.** Exploratory, short-lived, solo experiments may not justify the ceremony.

## Maturity Model

| Level | Meaning | Best Use |
| --- | --- | --- |
| Spec-first | A spec guides initial code generation, but code can drift later. | Prototypes and AI-assisted features. |
| Spec-anchored | Specs and code remain linked; tests or checks enforce alignment. | Most production systems. |
| Spec-as-source | Humans edit specs only; code is generated from them. | Mature teams with trusted generation pipelines. |

## Canonical Workflow

1. **Constitution**: Define project-wide rules for language, frameworks, dependencies, testing, security, and style.
2. **Specify**: Capture user stories, acceptance criteria, intent, and scope before choosing technical implementation.
3. **Clarify**: Resolve ambiguity before planning.
4. **Plan**: Decide architecture, data model, dependencies, and technical approach.
5. **Tasks**: Break the plan into small, independently shippable tasks.
6. **Implement**: Build each task and verify it against acceptance criteria.
7. **Analyze**: Check consistency across spec, plan, tasks, implementation, and tests.

## EARS Requirement Patterns

EARS means **Easy Approach to Requirements Syntax**. It gives repeatable patterns for writing acceptance criteria that humans and AI agents can interpret consistently.

| Pattern | Shape | Use |
| --- | --- | --- |
| Ubiquitous | The system shall... | General requirements. |
| Event-driven | When X happens, the system shall... | Behavior triggered by an event. |
| State-driven | While X is true, the system shall... | Behavior during a system state. |
| Unwanted behavior | If X occurs, then... | Error handling, limits, failures, abuse cases. |
| Optional | Where X applies, the system shall... | Feature flags, configuration, conditional behavior. |

## Tooling Mentioned

- **GitHub Spec Kit**: Open-source, model-agnostic reference implementation.
- **AWS Kiro**: Agentic IDE with guardrails and AWS integration.
- **Claude Code / cc-sdd**: Terminal-first SDD workflow with slash commands.
- **Cursor Plan Mode**: IDE-centered planning and diff review flow.
- **OpenSpec**: Lightweight Markdown and YAML approach.
- **BMAD-METHOD**: Community methodology using constitutions and multi-agent roles.
- **Tessl**: Compliance-focused workflows for regulated teams.
- **Google Antigravity**: Agent-first, specification-constrained autonomy.

Supporting older foundations include BDD tools, OpenAPI/Swagger, GraphQL SDL, Protocol Buffers, AsyncAPI, Pact, Specmatic, Simulink, and SCADE.

## Best Practices

- Commit a project constitution such as `AGENTS.md` or `.specify/memory/constitution.md`.
- Keep each feature in its own spec directory, such as `specs/004-magic-link/`.
- Keep specs short, usually 1-3 pages.
- Add explicit **out of scope** sections so the agent does not explore unrelated work.
- Write specs in domain language, not pseudo-code.
- Review the plan before task decomposition.
- Review tasks before implementation.
- Cite specs in commits and tasks.
- Run pre-flight checks for security, accessibility, observability, and consistency.

## Caveats

- Some criticism is valid: SDD can look like older contract-design or waterfall practices with new AI tooling around them.
- Specs can still be wrong, incomplete, or misleading.
- Generated code can still drift from intent unless tests and review gates enforce alignment.
- Over-specification turns the spec into a second implementation, which creates waste.
- Tooling can add ceremony without proportional value if the team does not need the rigor.

## Practical Takeaway

Use SDD when AI-assisted development needs durable alignment between product intent, implementation, and tests. For most teams, the strongest approach is **spec-anchored development**: write clear specs, turn acceptance criteria into enforceable checks, keep code maintainable, and use tools to preserve the loop rather than outsourcing judgment.
