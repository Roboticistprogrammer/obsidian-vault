# Frontend System Design

Source: [Frontend System Design Explained w/ Senior Engineer - theSeniorDev](https://www.youtube.com/watch?v=KuClyhvSzXk)

Supporting source: [Frontend System Design in the AI Era, Explained](https://theseniordev.com/senior-software-engineer-mental-models/frontend-system-design-explained/)

Note: YouTube exposed an English auto-caption track, but the caption API returned empty transcript bodies. This summary uses the exact video metadata plus the official guide page for the same video.

## Summary

Frontend system design is about more than React components. At senior level, the frontend includes team boundaries, deployment boundaries, API boundaries, rendering strategy, performance, shared UI systems, and increasingly AI-driven UI workflows.

The main message: as AI makes implementation cheaper, architecture and verification become more important. Strong frontend architecture should make independent shipping, consistent UI, reliable performance, and safe AI-assisted development easier.

## Core Concepts

### 1. Conway's Law and Team Shape

Systems tend to mirror the communication structure of the teams that build them.

Guidelines:

- Avoid large frontend teams where every change needs broad coordination.
- Prefer small feature teams that can own a vertical slice end to end.
- Keep a platform team for shared infrastructure, design systems, tooling, and guardrails.
- In AI-assisted development, optimize for code that is easy to verify, not just easy to generate.

### 2. Micro-Frontends and the Shell

Micro-frontends split a large frontend into independently owned and deployed applications.

Typical structure:

```text
Shell
  -> owns routing
  -> owns auth
  -> owns language / locale
  -> owns global state boundaries
  -> loads independent micro-frontends
```

Guidelines:

- Use micro-frontends when multiple teams must ship independently.
- Keep the shell responsible for cross-app concerns.
- Avoid leaking global CSS, global state, or hidden dependencies across micro-frontends.
- Do not use micro-frontends just because they sound advanced; they add operational and integration complexity.

### 3. Vertical Slices

A vertical slice combines the UI, frontend logic, backend services, and database/data ownership for one product domain.

Example:

```text
Product Team
  -> Product micro-frontend
  -> Product BFF / API
  -> Product services
  -> Product data
```

Guidelines:

- Prefer feature ownership from pixels to data when the product area changes frequently.
- Let teams release their own slice without waiting on unrelated teams.
- Treat modern senior frontend work as increasingly full-stack around product boundaries.

### 4. API Gateway

An API gateway handles shared edge concerns once instead of repeating them in every service.

Common responsibilities:

- HTTPS / TLS termination.
- Authentication.
- Caching.
- Rate limiting.
- Content negotiation.
- Routing into private services.

Guidelines:

- Put repeated edge behavior at the gateway.
- Keep internal services private where possible.
- Do not put core business logic in the gateway.

### 5. Backend for Frontend

A Backend for Frontend (BFF) is a backend owned around a frontend client or product experience.

Why it helps:

- The frontend does not wait for several backend teams to shape APIs exactly for the screen.
- The BFF aggregates multiple services into the data shape the client needs.
- Desktop and mobile can have different BFFs because they often need different data.

Guidelines:

- Use a BFF when the client must aggregate many backend services.
- Keep BFF APIs aligned with user interface needs.
- Avoid over-fetching and under-fetching.
- Learn API design and GraphQL basics because frontend teams often own this layer.

### 6. Load Balancing

When one server is not enough, run multiple identical instances behind a load balancer.

Guidelines:

- Scale horizontally rather than relying only on a bigger machine.
- Understand common strategies such as round robin and least connections.
- Know enough nginx and Docker Compose to model the idea locally.
- Expect cloud providers to manage most production load balancers.

### 7. Containers and Orchestration

Docker packages application code, runtime, and operating system dependencies into a portable image.

Orchestrators such as Kubernetes or AWS ECS run those images at scale.

Guidelines:

- Frontend engineers do not always need to operate Kubernetes, but they should know where containers sit in the architecture.
- Containers help when teams deploy many services with different runtimes.
- Orchestration handles parallel instances, restarts, and load distribution.

### 8. CDN

Latency has a physical limit. A CDN improves frontend performance by serving static assets from edge locations closer to users.

Guidelines:

- Serve JavaScript, CSS, images, fonts, and static assets from a CDN.
- Use cache-busting hashes from the bundler so long-lived caching stays safe.
- Understand cache hits, cache misses, and cache invalidation.
- CDN usage is one of the cheapest and highest-impact web performance wins.

### 9. Design Systems and Design Tokens

Independent teams drift visually unless they share a system.

A good design system includes:

- Design tokens for colors, spacing, typography, borders, and component primitives.
- Reusable accessible components.
- Shared documentation and examples.
- Versioning and ownership.

Guidelines:

- Use CSS custom properties or equivalent token systems for consistency.
- Treat the design system as architecture, not just UI polish.
- Feed design system rules into coding agents so generated UI remains consistent.
- Without a design system, AI-generated UI tends to drift across sessions.

### 10. Design-to-Code with MCP

The emerging AI workflow connects design tools and coding agents.

Typical flow:

```text
Figma
  -> MCP server exposes design data
  -> coding agent reads design system context
  -> agent generates UI inside the correct vertical slice
```

Guidelines:

- Know what an MCP server is and how it can expose design or tool context to agents.
- Keep design tokens structured so agents can use them reliably.
- Understand Tailwind as atomic CSS built from design tokens.

### 11. Monorepo

A monorepo places applications, shared tooling, design systems, configs, and packages in one repository.

Why it helps:

- Shared linting and TypeScript config.
- Shared build system.
- Easier dependency alignment.
- Less architectural drift.
- Better context for AI coding agents.

Guidelines:

- Use a monorepo when many frontend apps and shared packages need to evolve together.
- Keep ownership boundaries explicit even inside one repo.
- Use dependency graphs, affected builds, and shared quality gates.

### 12. MCP UI and LLM-Rendered Components

MCP UI lets an LLM answer with renderable UI instructions, not just plain text.

Example:

```text
User asks question
  -> LLM calls tools/resources
  -> response includes text + component instructions
  -> frontend renders cards, maps, pickers, or other UI
```

Guidelines:

- Treat chat as a container for interactive UI, not only text.
- Build a safe registry of components the model is allowed to render.
- Validate model output before rendering.
- This pattern is important for hybrid LLM/web applications.

### 13. Core Web Vitals and Critical Rendering Path

Important metrics:

- **LCP**: loading performance.
- **INP**: responsiveness to user input.
- **CLS**: visual stability.

These depend on the critical rendering path:

```text
HTML -> DOM
CSS -> CSSOM
DOM + CSSOM -> render tree
layout -> paint -> composite
```

Guidelines:

- Too much JavaScript or CSS hurts LCP.
- Slow event handlers and expensive re-renders hurt INP.
- Layout shifts hurt CLS.
- Measure performance instead of guessing.

### 14. Code Splitting and Lazy Loading

Large bundles make every page pay for code it does not need.

Guidelines:

- Split code by route first.
- Use dynamic imports for heavy features.
- Lazy-load content on navigation, click, or scroll.
- Keep dashboard-only code out of login-page bundles.

### 15. Rendering Strategies

Common rendering strategies:

- **CSR**: client-side rendering; simple SPA model, but can create white-screen loading.
- **SSG**: static site generation; pre-render at build time.
- **ISR**: incremental static regeneration; rebuild changed pages after deployment.
- **SSR**: server-side rendering; render per request, then hydrate on the client.

Guidelines:

- Default to the simplest rendering strategy that satisfies the product.
- Use SSR when SEO or initial performance requires it.
- Avoid SSR when it only adds complexity without product value.
- Understand hydration cost.

### 16. Real-Time UI: Polling, WebSockets, and SSE

Options:

- **Polling**: simple but can waste server resources.
- **WebSockets**: bidirectional and useful for peer-like real-time apps such as chat.
- **Server-Sent Events (SSE)**: one request up, continuous stream down.

Guidelines:

- Use polling for simple low-frequency updates.
- Use WebSockets when both client and server need to send frequent messages.
- Use SSE for LLM streaming and asymmetric flows where the user sends one request and receives many chunks back.

## Practical Takeaway

Frontend system design is the architecture of the whole user-facing delivery system. The important questions are:

- How are frontend teams and ownership boundaries shaped?
- Can each product slice ship independently?
- Where do shared concerns live: shell, gateway, BFF, platform, or design system?
- How are assets cached and delivered globally?
- Which rendering strategy fits the product?
- How do we keep performance measurable?
- How do AI agents consume design, code, and component context safely?

Senior frontend work is moving toward architecture, platform thinking, API design, and AI-aware systems, not just component implementation.
