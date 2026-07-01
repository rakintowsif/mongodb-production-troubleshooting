# 🚨 RCA-001: No Primary During DR Outage

When the DR data center went completely offline, the replica set lost its **primary** — even though half of all nodes were still healthy and online. This RCA covers the topology flaw that caused it, and the fix that prevents it from happening again.

## TL;DR

| | Before | After |
|---|---|---|
| Topology | 3 DC + 3 DR, no arbiter | 2 DC + 2 DR + 1 arbiter (AWS) |
| Voting members | 6 | 5 |
| Votes needed for majority | 4 | 3 |
| Nodes left alive when DR is fully down | 3 | 3 (2 DC + arbiter) |
| Primary electable during DR outage? | ❌ No | ✅ Yes |

## Documents

1. [PREVIOUS-STATUS.md](PREVIOUS-STATUS.md) — the original 6-node topology and why it looked fine on paper
2. [PROBLEM.md](PROBLEM.md) — what happened when DR went down, and the root-cause math
3. [SOLUTION.md](SOLUTION.md) — the new 5-node topology and why it fixes the majority math
4. [CURRENT-STATUS.md](CURRENT-STATUS.md) — live verification of the fixed cluster

## Related
- [RCA-003 — Write Availability During DR Outage](../RCA-003-write-availability-during-dr-outage/README.md) — the write-concern fix applied on top of this same topology change
- [RCA-002 — Replica Set Member Removal Blocked](../RCA-002-replica-set-member-removal-failure/README.md)

---
**Status:** Implemented in Production · **Environment:** Production
