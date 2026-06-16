# MongoDB Write Availability During DR Outage (RCA-003)

## Problem
DR data center offline → default `w: "majority"` needs 3 nodes, only 2 available → **writes blocked**.

## Cluster Layout
```
Primary DC              DR DC               AWS Cloud
mongodb1 ✓              mongodb4 ✗           mongodb9 (Arbiter)
mongodb2 ✓              mongodb5 ✗           
─────────────────────────────────────────────────────
Available: 2 nodes | Required: 3 | Result: BLOCKED ❌
```

## Root Cause
- 5 voting members: 4 data nodes + 1 arbiter
- Default: `w: "majority"` = 3 acknowledgments required
- Arbiter can't acknowledge writes → need 3 data nodes
- DR offline = 2 data nodes only → **2 < 3 = write hang**

## 4. The Permanent Solution

## Solution
Execute on Primary node:

```javascript
db.adminCommand({
  setDefaultRWConcern: 1,
  defaultWriteConcern: { w: 2, wtimeout: 10000 }
});
```

### Technical Impact of the Parameters:
| Parameter | Value | Purpose |
|-----------|-------|---------|
| **w** | 2 | Requires 2 data nodes (Primary DC) to acknowledge writes |
| **wtimeout** | 10000ms | Timeout circuit breaker (prevents infinite hangs) |

**Result:** Writes now succeed with 2-node acknowledgment ✅

## 5. Verification Matrix
## Verification
Confirm with:
```javascript
db.adminCommand({ getDefaultRWConcern: 1 })
```

### Expected Valid Output:
```json
{
  "defaultReadConcern": {
    "level": "local"
  },
  "defaultWriteConcern": {
    "w": 2,
    "wtimeout": 10000
  },
  "defaultWriteConcernSource": "global",
  "ok": 1
}
```

### Output Field Explanations
✅ Configuration is permanent (survives cluster restarts)

---

**Document Status:** ✅ Implemented in Production  
**Last Updated:** 2026-06-16  
**Applicable Versions:** MongoDB 5.0+  
**Environments:** Staging, Production
