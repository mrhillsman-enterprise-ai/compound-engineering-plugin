---
name: slfg
description: "[DEPRECATED] Compatibility wrapper that routes to lfg with swarm mode enabled."
argument-hint: "[feature description]"
disable-model-invocation: true
---

`slfg` is deprecated.

Do not maintain a separate orchestration contract here.

Behavior:

1. Announce briefly that `slfg` is deprecated and that `lfg` now owns the autopilot contract.
2. Preserve the user's feature description exactly.
3. Immediately route to `lfg` using the same input, with swarm execution enabled.
4. Do not duplicate routing logic, manifest logic, or downstream skill-calling rules here. `lfg` is the source of truth.

When users ask for swarm explicitly, prefer `/lfg ...` with swarm mode going forward.
