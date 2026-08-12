# Heartbeat watchdog setup

The example watchdog is the current ComicPile implementation, preserved with its real worker names,
schedule slots, registry issue number, thresholds, and alert behavior. It is stored under
`examples/` intentionally, so it does not execute in this repository.

To reuse it elsewhere:

1. Copy `examples/factory-heartbeat-watchdog.yml` to
   `.github/workflows/factory-heartbeat-watchdog.yml` in the repository being monitored.
2. Create one permanent registry issue and one permanent comment per scheduled worker.
3. Make each scheduled worker edit its own comment at the start and end of every run. The comment
   must contain `<!-- factory-heartbeat:v1 worker=<durable-worker-id> -->` and an
   `Outcome:` line. Use `Outcome: running` at startup and replace it with the final outcome.
4. Replace `REGISTRY_ISSUE` with that issue number.
5. Replace every entry in `expectedWorkers` with the durable worker ID, display name, and local
   schedule slot used by the new factory.
6. Tune `STALE_MINUTES` to exceed the normal interval between scheduled runs and tune
   `RUNNING_MINUTES` to exceed a normal run duration.
7. Confirm the workflow has `issues: write`, run it manually, and verify both behaviors:
   an unhealthy worker creates or reopens one owner-assigned alert issue, and recovery closes it.

Telemetry is observation only. Heartbeat edits must never count as substantive factory progress,
must never reserve a worker, and must never hide a delivery failure.
