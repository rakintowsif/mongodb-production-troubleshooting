# The Problem: No Primary During DR Outage

## Incident
The DR data center went **completely offline** (all 3 DR nodes unreachable at once). The DC site was untouched — all 3 DC nodes stayed healthy.

Despite 3 out of 6 nodes still being fully healthy, the replica set had **no primary**. Every DC node showed as `SECONDARY`, and no election could succeed.

## Root Cause
```
Total voting members : 6  (3 DC + 3 DR)
Majority required    : floor(6 / 2) + 1 = 4
Nodes alive (DR down): 3  (DC only)

3 < 4  →  majority unreachable  →  no primary can be elected
```

A replica set can only elect (or keep) a primary if it can get **votes from a majority of all voting members** — not a majority of *currently reachable* members. Because the previous topology had exactly 6 voting members split 3/3, losing one entire site always left exactly 3 votes: one short of the 4 needed, no matter which site failed.

## Impact
- No primary → **no writes accepted anywhere in the cluster**
- Application fully write-locked for the duration of the DR outage
- The DC site — fully healthy, fully in sync — was still unable to serve as primary, purely due to vote count

## Why "3 healthy nodes" Wasn't Enough
This is the counter-intuitive part: node *health* didn't matter here. MongoDB's election quorum is based on the **configured** number of voting members, not how many happen to be reachable. A perfectly healthy 3-node DC couldn't self-elect a primary because the replica set's math still required a 4th vote that no longer existed.

Fix and math for the new topology: [SOLUTION.md](SOLUTION.md)
