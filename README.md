# factory

This repository is a verbatim snapshot of the autonomous software-factory setup currently used for ComicPile.

Nothing in the copied prompt or referenced policy files has been generalized or cleaned up. Project-specific names, issue numbers, labels, commands, paths, infrastructure assumptions, and workflow quirks are intentionally preserved.

## Files

- [`FACTORY_PROMPT.md`](FACTORY_PROMPT.md) — verbatim canonical scheduled-task template. Replace
  the worker number, durable ID, and call sign for each scheduled worker; those identity fields are
  the only intended differences.
- [`docs/AUTONOMOUS_FACTORY_POLICY.md`](docs/AUTONOMOUS_FACTORY_POLICY.md) — verbatim current-main ComicPile autonomous factory policy.
- [`docs/ISSUE_EXECUTION_PROTOCOL.md`](docs/ISSUE_EXECUTION_PROTOCOL.md) — verbatim current-main ComicPile issue execution protocol.
- [`docs/FACTORY_GITHUB_VISIBILITY.md`](docs/FACTORY_GITHUB_VISIBILITY.md) — verbatim current-main ComicPile factory visibility rules.
- [`AGENTS.md`](AGENTS.md) — verbatim current-main ComicPile repository agent instructions.
- [`examples/factory-heartbeat-watchdog.yml`](examples/factory-heartbeat-watchdog.yml) — the
  external GitHub Actions watchdog used to detect missing, stale, or stuck scheduled workers.
- [`docs/HEARTBEAT_WATCHDOG_SETUP.md`](docs/HEARTBEAT_WATCHDOG_SETUP.md) — the minimum
  repository-specific substitutions required before reusing the watchdog.

The purpose of this repository is to show the actual working system rather than a sanitized template.

The snapshot currently tracks ComicPile factory policy Version 20 and Durable Resume Packet V1.

ComicPile automatically copies the five canonical contract files to a dedicated synchronization
branch. This repository opens and squash-merges a pull request for that branch automatically, so
public documentation follows the private source without publishing application code. Operational
examples are copied separately and kept outside `.github/workflows/` so cloning this repository
cannot accidentally activate ComicPile-specific monitoring.
