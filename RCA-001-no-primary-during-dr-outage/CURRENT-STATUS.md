# Current Status: Verified Healthy

## Members
| Host | Location | Role | Votes |
|---|---|---|---|
| mongodb1:27017 | DC | PRIMARY | 1 |
| mongodb2:27017 | DC | SECONDARY | 1 |
| mongodb4:27017 | DR | SECONDARY | 1 |
| mongodb5:27017 | DR | SECONDARY | 1 |
| mongodb9:27017 | AWS | ARBITER | 1 |

## Live Quorum Numbers
```javascript
mongodb-replica [direct: primary] admin> rs.status()
votingMembersCount: 5
majorityVoteCount: 3
writeMajorityCount: 3
writableVotingMembersCount: 4
```

## Write Concern
```javascript
mongodb-replica [direct: primary] admin> db.adminCommand({ getDefaultRWConcern: 1 })
{
  defaultReadConcern: { level: 'local' },
  defaultWriteConcern: { w: 2, wtimeout: 10000 },
  defaultWriteConcernSource: 'global',
  ok: 1
}
```

## What This Confirms
- ✅ 5 voting members, majority = 3 — matches the design in [SOLUTION.md](SOLUTION.md)
- ✅ `defaultWriteConcernSource: 'global'` — write concern is explicitly pinned, not implicit
- ✅ `w: 2` — writes only need 2 data-node acknowledgments, achievable even with one site down
- ✅ Arbiter (`mongodb9`) healthy and voting, holds no data (`writableVotingMembersCount: 4`, one less than `votingMembersCount: 5`)

**Captured:** 2026-06-17
