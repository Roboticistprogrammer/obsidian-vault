# Clean Architecture Tips

Source: [What Rewriting a Legacy Frontend Taught Me About Software Architecture - WebDevPlaybook](https://youtu.be/WBAJ8y45tbI?si=813C9qQec7_AbMGu)

## Summary

The video is about a real frontend migration, not abstract Clean Architecture theory. The main lesson is that architecture is useful when it creates clear boundaries, reduces duplication, and lets teams move without constantly breaking each other.

Clean Architecture helped most where business logic needed to survive framework changes and be shared across apps.

## Highlighted Tips

- **Listen to what the codebase is telling you.** Repeated components, duplicated logic, and teams solving the same problem twice are architecture signals, not just cleanup tasks.
- **Do not lock business logic inside framework-specific code.** If important rules live inside Angular services, React components, or UI-specific helpers, migration becomes harder.
- **Separate domain logic from UI logic.** The domain layer should contain business rules that can be reused by web apps, CLIs, tests, and future frameworks.
- **Separate repos can create accidental boundaries.** Multi-repo setups may look organized, but they often make sharing code harder and push teams toward duplication.
- **Use a monorepo when shared ownership and shared code matter.** A monorepo can make common libraries, domain logic, and design system components easier to discover and reuse.
- **Do not create shared code without clear ownership.** Shared packages need maintainers, versioning rules, documentation, and review responsibility.
- **Build shared abstractions around stable business concepts.** Avoid sharing random utilities too early; share the parts that multiple apps truly depend on.
- **Use a design system to stop UI duplication.** Reusable components reduce visual drift and make large migrations easier.
- **Treat the design system as a product.** It needs documentation, examples, contribution rules, and active maintenance.
- **Clean Architecture proves itself during change.** The value appears when a framework, app, or delivery channel changes but the domain model and business rules stay reusable.
- **Architecture cannot fix team misalignment.** If teams do not communicate or agree on ownership, code structure alone will not solve the problem.
- **Be pragmatic.** Do not force Clean Architecture everywhere. Use it where boundaries, reuse, testability, and migration safety are worth the extra structure.

## Practical Guidelines

- Start by finding duplication and dependency pain.
- Move business rules out of components and framework services.
- Create shared packages only for stable, repeatedly used behavior.
- Keep app-specific UI code close to the app.
- Keep cross-app domain logic in a shared domain layer.
- Prefer boring structure that teams can understand quickly.
- Make ownership explicit for every shared module.
- Use architecture to reduce coordination cost, not to create ceremony.

## Practical Takeaway

Clean Architecture is most useful when a system must survive change. In frontend migrations, the goal is not to draw perfect layers; the goal is to keep business logic independent from frameworks, reduce duplication across teams, and make shared code easy to trust.
