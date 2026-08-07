# factory

A reusable pattern for running a small autonomous software-delivery factory on top of GitHub and scheduled AI workers.

The core idea is simple:

> Let probabilistic workers make engineering decisions inside deterministic delivery gates.

GitHub is the durable control plane. Issues are work orders. Pull requests are work products. CI, tests, types, review, and exact-head merge checks decide whether work is allowed to advance.

This repository contains a generalized version of the policies used by a real multi-worker software factory.

## Files

- [`FACTORY_PROMPT.md`](FACTORY_PROMPT.md) — a portable prompt for a scheduled autonomous worker.
- [`docs/AUTONOMOUS_FACTORY_POLICY.md`](docs/AUTONOMOUS_FACTORY_POLICY.md) — canonical lifecycle, selection, concurrency, review, repair, and merge policy.
- [`docs/ISSUE_EXECUTION_PROTOCOL.md`](docs/ISSUE_EXECUTION_PROTOCOL.md) — how an agent should execute one GitHub issue from claim through closure.
- [`docs/FACTORY_GITHUB_VISIBILITY.md`](docs/FACTORY_GITHUB_VISIBILITY.md) — optional label and marker conventions for seeing worker ownership and stage in GitHub.
- [`AGENTS.md`](AGENTS.md) — a minimal repository-level agent contract to customize for your project.

## Recommended architecture

```text
GitHub issue
    ↓
scheduled worker heartbeat
    ↓
claim / implement / repair
    ↓
pull request
    ↓
focused local validation
    ↓
CI + types + tests + coverage + E2E as configured
    ↓
review feedback
    ↓
exact-head revalidation
    ↓
gated merge
    ↓
verify issue closure
```

The workers can be ephemeral. The state should not be.

Use GitHub issues, comments, labels, pull requests, commits, review threads, and CI as durable shared state so a later worker can reconstruct what happened without relying on conversation memory.

## The most important rule

A worker may choose **how** to implement something. It does not get to decide by assertion that the implementation works.

Your repository should provide deterministic or independently verifiable gates such as:

- static types;
- linting and formatting;
- unit and integration tests;
- coverage thresholds;
- schema or generated-contract validation;
- browser/E2E tests where behavior requires them;
- CI required checks;
- independent automated or human review;
- protected-branch rules;
- expected-head-SHA merge protection.

Do not weaken those gates to make autonomous delivery easier. The gates are what make autonomy useful.

## Backlog shape matters

Treat the backlog as an executable work queue, not merely a list of ideas.

A useful hierarchy is:

```text
EPIC / CAMPAIGN
    ↓
FACTORY-EXECUTABLE ISSUE
    ↓
PR
```

A factory-executable issue should have one coherent outcome, explicit dependencies, objective acceptance criteria, and a completion state another worker can verify without interpreting intent.

A substantial issue is fine. A mini-project wearing an issue number is not.

## Multiple workers

Run several workers on staggered schedules if your platform permits it. Give each a durable worker identity and coordinate through GitHub leases/markers rather than shared conversational memory.

Workers should:

1. re-fetch shared state before claims and writes;
2. avoid duplicating active work;
3. prefer separate coherent issues when capacity exists;
4. treat waiting as queue state, not worker ownership;
5. revalidate after every push;
6. merge only the exact head they actually evaluated.

## Setup

1. Copy the files in this repository into the repository you want the workers to maintain.
2. Customize `AGENTS.md` with project-specific engineering constraints and validation commands.
3. Customize the policy where your repository has different merge, review, E2E, release-note, or deployment requirements.
4. Define labels and durable markers if you want GitHub-native factory visibility.
5. Create one or more scheduled AI tasks using `FACTORY_PROMPT.md`, giving each a unique worker ID and owner label.
6. Make only well-scoped issues eligible for autonomous implementation.
7. Keep humans responsible for product direction and ambiguous architecture decisions; let factories own bounded execution.

## Safety model

The factory should fail closed around uncertainty.

It may autonomously implement, test, review, repair, push, and merge only where the repository policy provides objective gates. It should escalate when product intent, destructive operations, credentials, migrations, or architectural direction cannot be established safely from durable repository context.

## Origin

This repository is a project-neutral extraction of the autonomous delivery policies developed while running multiple scheduled AI software workers against a real application backlog. The project-specific names, issue numbers, product rules, and infrastructure assumptions have been removed so the pattern can be reused elsewhere.
