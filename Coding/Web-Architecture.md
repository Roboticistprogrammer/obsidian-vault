# Web Architecture

Source: [Web Application Architecture: Full Request-Response Lifecycle - ByteMonk](https://youtu.be/xv0Be4QfkH0?si=PmZ9GHhTGahv8i9n)

## Summary

A modern web application request passes through multiple layers before it reaches business logic and returns a response. Each layer has a specific responsibility: finding the server, serving cached content, blocking attacks, routing traffic, balancing load, executing backend logic, reading or writing data, and monitoring the whole path.

The main idea is that scalable web architecture is not just "frontend calls backend." It is a chain of infrastructure and application components designed for speed, security, availability, and maintainability.

## Full Request-Response Lifecycle

1. **User action**: A user clicks, submits a form, opens a page, or triggers an API request.
2. **DNS resolution**: The domain name is translated into an IP address.
3. **CDN / CloudFront**: Static or cached content is served from an edge location close to the user.
4. **Web Application Firewall (WAF)**: Suspicious or malicious traffic is filtered before it reaches the application.
5. **Reverse proxy / Nginx**: Requests are forwarded to internal services and dynamic content may be cached.
6. **Load balancer**: Traffic is distributed across multiple application instances for availability and scale.
7. **API Gateway**: Requests are validated, authenticated, rate-limited, and routed to the correct backend service.
8. **Backend services**: Microservices or application services execute business logic.
9. **Database layer**: Services read from or write to SQL or NoSQL databases.
10. **Supporting services**: Authentication, caching, monitoring, logging, and other services support the request.
11. **Response**: The result travels back through the same general path to the user's browser or app.

## Important Components

### DNS

DNS maps human-readable domains to machine-usable IP addresses.

Guidelines:

- Keep DNS records simple and documented.
- Use managed DNS for reliability.
- Use appropriate TTL values: shorter TTLs make failover easier, longer TTLs reduce lookup overhead.

### CDN / CloudFront

A CDN caches content near users so the origin servers do less work and users get faster responses.

Guidelines:

- Cache static assets such as images, CSS, JavaScript, videos, and downloadable files.
- Use cache-control headers intentionally.
- Avoid sending every request to the origin if the content can safely be cached.
- Invalidate or version assets when content changes.

### Web Application Firewall

A WAF protects the application before requests reach backend infrastructure.

Guidelines:

- Block common attacks such as SQL injection, cross-site scripting, and suspicious request patterns.
- Use rate limits for abusive traffic.
- Treat WAF rules as a first layer of defense, not the only security layer.

### Reverse Proxy / Nginx

A reverse proxy sits in front of application servers and forwards traffic internally.

Guidelines:

- Use it for routing, TLS termination, compression, and caching where appropriate.
- Keep backend services private behind the proxy.
- Cache dynamic content only when it is safe for correctness and privacy.

### Load Balancer

A load balancer distributes requests across multiple servers or containers.

Guidelines:

- Run multiple application instances so one failure does not take the service down.
- Use health checks so traffic is only sent to healthy instances.
- Choose a balancing strategy that fits the workload.
- Design services to be stateless when possible, so requests can go to any instance.

### API Gateway

An API gateway is the entry point for backend APIs.

Guidelines:

- Validate incoming requests before they reach services.
- Centralize authentication, authorization checks, throttling, and request routing.
- Keep gateway logic focused on cross-cutting concerns.
- Avoid turning the gateway into a place for core business logic.

### Backend Services

Backend services handle the actual business operations requested by the user.

Guidelines:

- Split services by business capability, not by random technical layers.
- Keep service boundaries clear.
- Make service APIs explicit and versioned when needed.
- Use async messaging for slow or background work when the user does not need an immediate result.

### Database Layer

The database stores persistent application data.

Guidelines:

- Use SQL databases when relationships, transactions, consistency, and structured queries matter.
- Use NoSQL databases when flexible schemas, high write throughput, or key-value/document access patterns are a better fit.
- Do not choose SQL vs NoSQL by popularity; choose based on query patterns and consistency needs.
- Add indexes based on real access patterns.
- Keep database calls efficient because this layer is often a major latency source.

### Supporting Services

Authentication, caching, monitoring, and logging are essential parts of production architecture.

Guidelines:

- Use authentication to identify users and protect private routes.
- Use caching to reduce repeated database and service calls.
- Use monitoring to detect latency, errors, traffic spikes, and service failures.
- Use logs and traces to understand what happened during a request.

## Practical Design Rules

- Design the request path deliberately; every layer should have a clear job.
- Cache as close to the user as correctness allows.
- Protect public entry points before traffic reaches internal services.
- Keep services horizontally scalable by avoiding unnecessary local state.
- Add load balancing and health checks before traffic grows large enough to make outages painful.
- Use the API gateway for validation, routing, authentication, authorization, and rate limiting.
- Keep business logic in backend services, not in infrastructure routing layers.
- Choose databases based on data model, access patterns, consistency, and scalability needs.
- Add observability from the beginning: logs, metrics, alerts, and tracing.
- Optimize the slowest part of the lifecycle first instead of guessing.

## Architecture Flow

```text
User
  -> DNS
  -> CDN / CloudFront
  -> WAF
  -> Reverse Proxy / Nginx
  -> Load Balancer
  -> API Gateway
  -> Backend Service
  -> Database / Cache / Auth / Monitoring
  -> Response back to User
```

## Practical Takeaway

A scalable web application is built as a layered request pipeline. The browser does not simply call one server; the request moves through DNS, caching, security, routing, load distribution, API control, backend logic, storage, and observability. Good architecture assigns each concern to the right layer and keeps the system fast, secure, and resilient.
