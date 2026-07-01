# 🚨 RCA-002: Replica Set Member Removal Blocked

## Overview
While removing a member from a MongoDB replica set, the reconfiguration operation failed because MongoDB blocked changes to the voting members.

## Root Cause
The replica set contained:
- 5 voting members (including 1 arbiter)
- An implicit `defaultWriteConcern` of `majority`

With an arbiter present, MongoDB requires the cluster-wide `defaultWriteConcern` to be explicitly configured before allowing replica set reconfiguration.

## Resolution
Explicitly configured the cluster-wide write concern:
```javascript
db.adminCommand({
  setDefaultRWConcern: 1,
  defaultWriteConcern: {
    w: "majority",
    wtimeout: 0
  }
})
```

**Note:** The write concern remained `majority`. Only its configuration changed from implicit to explicit.

## Verification
```javascript
db.adminCommand({ getDefaultRWConcern: 1 })   // source: "global"
rs.remove("mongodb5:27017")                   // Success
```

## Key Takeaway
When a MongoDB replica set includes an arbiter, always configure `defaultWriteConcern` explicitly using `setDefaultRWConcern`. This prevents replica set reconfiguration from being blocked during future maintenance operations.
