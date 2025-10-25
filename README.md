# CS-230-10271-M01-Operating-Platforms
CS-230-10271-M01 Operating Platforms


1) Client + requirements (brief)

Client: The Gaming Room (via Creative Technology Solutions).

Ask: Take the existing Android-only game Draw It or Lose It and deliver a web-based, multi-platform version available in modern browsers.

Key functional needs: unique game/team names; multiple players per team; exactly one in-memory instance per game; four 1-minute rounds with the drawing finishing at 30s and a 15s steal window.

Technical direction: browser client (HTML5/JS/TS) + stateless REST/JSON service, optional WebSockets for real-time cues, relational DB as source of truth; recommend Linux for server hosting. 

CS 230 Project Software Design …

2) What I did well

Made tradeoffs explicit. I clearly justified Linux for hosting, REST for commands, and WebSockets for push.

Connected requirements to mechanisms. Uniqueness → DB UNIQUE constraints; single game instance → per-game coordinator + distributed lock; scalability → stateless containers behind a load balancer.

Solid domain model. Clean separation of Game/Team/Player with composition for rounds and encapsulated invariants.

3) Helpful parts of the design-doc process

Requirements → constraints → architecture path. Writing constraints (idempotency, atomicity, TLS, JWT) early prevented accidental complexity later.

Operational thinking. Forcing myself to specify observability, CI/CD, and recovery flows improved the API shape and error handling.

Naming + uniqueness upfront. Modeling name availability checks avoided racy “create/join” UX later.

4) If I could revise one part

I’d expand the API and runtime behavior with:

Sequence/state diagrams for timers, steals, and reconnects.

Explicit error/latency budgets (e.g., timing tolerances during round transitions).

Contract tests and an OpenAPI spec to make the server–client boundary verifiable.

5) Translating user needs into design (and why it matters)

Players need fast rounds, clear turns, and fair steals. I encoded that as authoritative timing on the server, push updates to every client, and rules enforced behind methods (e.g., only one active round, validated team membership). Considering user needs early prevents UX surprises (race conditions on naming, out-of-sync timers) and keeps the game fair and responsive under load.

6) My approach + future techniques

Start with scenarios & acceptance tests. Write user stories like “as a captain, create a game with an available name” and define pass/fail criteria.

Model first, code second. Domain model + sequence diagrams, then derive APIs.

Architectural Decision Records (ADRs). Capture tradeoffs (e.g., REST+WS vs. pure gRPC) for future maintainers.

Risk-first spikes. Prototype the tightest loops (timers, reconnect/replay) before broad implementation.

Quality baked in. Property tests for rules, load tests for round churn, chaos testing for node loss, and observability (logs/metrics/traces) from day one.

Security & privacy by design. Threat modeling (STRIDE), least-privilege IAM, short-lived tokens, and input validation as non-negotiables.
