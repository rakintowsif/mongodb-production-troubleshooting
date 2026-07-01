# The Solution: 5-Node Topology With a Neutral Arbiter

## New Layout
```
Data Center (DC)        Disaster Recovery (DR)      AWS Cloud
  mongodb1 ✓               mongodb4 ✓                 mongodb9 (Arbiter)
  mongodb2 ✓               mongodb5 ✓
──────────────────────────────────────────────────────────────
Voting members: 5 (2 DC + 2 DR + 1 arbiter, hosted in a 3rd location)
```

## Why This Fixes the Math
Dropping one data node per site and adding a single arbiter in a third, independent location (AWS) changes the majority calculation:
```
Total voting members : 5  (2 DC + 2 DR + 1 arbiter)
Majority required    : floor(5 / 2) + 1 = 3
Nodes alive (DR down): 3  (2 DC + arbiter)

3 = 3  →  majority reachable  →  primary can be elected
```

The arbiter is the key piece: it **holds no data and can't acknowledge writes**, but it **does get a vote**. Placing it in a third, independent location means it survives the loss of *either* the DC or the DR site — so whichever site goes down, the surviving site plus the arbiter still adds up to a majority.

## Change Applied
```javascript
db.adminCommand({
  setDefaultRWConcern: 1,
  defaultWriteConcern: { w: 2, wtimeout: 10000 }
})
```
The replica set was reconfigured (`rs.reconfig()`) to the new 5-member layout, and the default write concern was updated to `w: 2` so that writes only need acknowledgment from 2 data-bearing nodes — matching what's actually available when one site is down. (Full detail on this part of the fix: [RCA-003](../RCA-003-write-availability-during-dr-outage/README.md).)

## Result
- Losing DC **or** DR now still leaves a majority (2 data nodes + arbiter = 3 votes)
- A primary can always be elected as long as one full site is up
- Verified against the live cluster in [CURRENT-STATUS.md](CURRENT-STATUS.md)
