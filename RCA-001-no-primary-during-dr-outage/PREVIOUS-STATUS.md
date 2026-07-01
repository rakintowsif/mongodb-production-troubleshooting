# Previous Status: 6-Node Topology (No Arbiter)

## Layout
```
Data Center (DC)          Disaster Recovery (DR)
  mongodb-dc-1 ✓             mongodb-dr-1 ✓
  mongodb-dc-2 ✓             mongodb-dr-2 ✓
  mongodb-dc-3 ✓             mongodb-dr-3 ✓
────────────────────────────────────────────
Voting members: 6 (3 DC + 3 DR) · No arbiter
```
> Node names above are illustrative — the exact previous `rs.conf()` was not captured before the topology was changed. The counts (3 DC / 3 DR / 0 arbiter / 6 total votes) are accurate.

## Why It Looked Fine
- Every node was a full, data-bearing, voting member.
- DC and DR were symmetric (3 and 3) — an intentionally "balanced" design.
- Under normal conditions, all 6 nodes were healthy and replicating.

## The Hidden Flaw
With 6 voting members, MongoDB needs a **majority of 4** votes to elect a primary or commit a write:
```
majority = floor(6 / 2) + 1 = 4
```
The design was symmetric per-site (3 vs 3), but the *math* was not: if either entire site went down, only 3 votes remained — one short of the 4 needed.

This flaw was invisible during normal operation and only surfaced when an entire site failed at once — see [PROBLEM.md](PROBLEM.md).
