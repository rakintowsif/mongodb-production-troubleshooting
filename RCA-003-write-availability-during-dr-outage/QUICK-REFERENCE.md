# RCA-003: Quick Reference Cheat Sheet

## The Problem in 30 Seconds
- **What:** Write operations hung indefinitely when DR data center went offline
- **Why:** Default write concern required 3 node acknowledgments, but only 2 data nodes were available
- **Impact:** Complete application write lockout, PHP-FPM worker exhaustion, cascading timeouts

---

## The Solution in 10 Seconds
```javascript
db.adminCommand({
  setDefaultRWConcern: 1,
  defaultWriteConcern: { w: 2, wtimeout: 10000 }
});
```
**Result:** Writes now succeed with 2-node acknowledgment from Primary DC ✅

---

## Cluster Layout
```
Primary DC (us-east-1)     Disaster Recovery (us-west-2)     AWS Cloud
   mongodb1 ✓                mongodb4 ✗                      mongodb9
   mongodb2 ✓                mongodb5 ✗                     (Arbiter)
   ↓                         ↓                              ↓
   2 data nodes              2 data nodes (DOWN)            1 voting node
   (Available)               (Offline)                      (No storage)
```

---

## Before vs After

| Metric | Before | After |
|--------|--------|-------|
| Write Concern | `w: "majority"` (3 nodes) | `w: 2` (2 nodes) |
| Available Nodes | 2 data + 1 arbiter | 2 data + 1 arbiter |
| Requirement Met? | ❌ NO | ✅ YES |
| Write Status | 🔴 BLOCKED | 🟢 OPERATIONAL |
| Timeout Value | `wtimeout: 0` (infinite) | `wtimeout: 10000` (10 sec) |
| Circuit Breaker | ❌ No | ✅ Yes |

---

## Key Commands Reference

### Verify Current Configuration
```javascript
db.adminCommand({ getDefaultRWConcern: 1 })
```

### Apply the Fix
```javascript
db.adminCommand({
  setDefaultRWConcern: 1,
  defaultWriteConcern: { w: 2, wtimeout: 10000 }
});
```

### Check Replica Set Status
```javascript
rs.status()
```

### Test Write Operation
```javascript
db.test.insertOne({ 
  test: "write_test", 
  timestamp: new Date() 
})
```

---

## Why w: "majority" Failed

**Math:** 5 voting members → majority = 3 members  
**Problem:** Arbiters can't acknowledge writes → need 3 data nodes  
**Reality:** Only 2 data nodes available (DR down) → 2 ≠ 3 → ❌ BLOCKED

## Why w: 2 Works

**Requirement:** 2 data-bearing nodes  
**Available:** 2 data nodes (mongodb1, mongodb2 in Primary DC)  
**Result:** 2 = 2 → ✅ SUCCESS

---

## Impact Timeline

| Time | Event | Status |
|------|-------|--------|
| T0 | DR data center offline | ⚠️ Incident starts |
| T+5min | Writes timeout | 🔴 CRITICAL |
| T+15min | PHP-FPM workers exhausted | 🔴 CRITICAL |
| T+30min | Application restart ordered | 🔴 CRITICAL |
| T+45min | RCA identifies root cause | 🟡 INVESTIGATING |
| T+60min | Fix implemented on Staging | 🟡 TESTING |
| T+90min | Fix implemented on Production | 🟢 RESOLVED |

---

## Testing Checklist

- [ ] Pre-fix: Verify `w: "majority"` is in effect
- [ ] Apply fix via `setDefaultRWConcern` command
- [ ] Verify fix with `getDefaultRWConcern` command
- [ ] Insert test document - should succeed
- [ ] Bulk insert test - should succeed
- [ ] Check replication lag - should be < 100ms
- [ ] Wait 10 seconds with no writes - no timeout
- [ ] Restart MongoDB - verify configuration persists
- [ ] Simulate DR failure - writes should still work
- [ ] Check logs for write concern errors - should be none

---

## Troubleshooting

| Problem | Cause | Solution |
|---------|-------|----------|
| Writes still timeout | Replication lag > 10s | Increase `wtimeout` or fix network |
| Cannot apply fix | Not connected to primary | Verify connection to primary node |
| Change lost after restart | Typo in command | Re-apply with correct syntax |
| Writes slower than before | 2-node sync takes time | Normal - network latency matters now |
| Replication lag increasing | Secondary falling behind | Check disk I/O and network on secondary |

---

## Important Notes

✅ **Preserves:** Election safety, cluster structure, data integrity  
⚠️ **Requires:** Healthy Primary DC with both data nodes running  
🔄 **Automatic:** Configuration survives cluster restarts  
📊 **Monitoring:** Track replication lag and write timeout errors  

---

## Related RCAs

- **RCA-001:** No Primary During DR Outage
  - Focus: Election mechanism and quorum calculation
  
- **RCA-002:** Replica Set Member Removal Failure  
  - Focus: Member state transitions and removal safety

- **RCA-003:** Write Availability During DR Outage (THIS DOCUMENT)
  - Focus: Write concern configuration and quorum mismatch

---

## Files in This RCA

1. **README.md** - Complete technical documentation
2. **IMPLEMENTATION-GUIDE.md** - Step-by-step procedures and scripts
3. **CLUSTER-CONFIGURATION.md** - Detailed cluster setup and topology
4. **QUICK-REFERENCE.md** - This file (quick lookup)

---

## Quick Copy-Paste Commands

### For Emergency Application of Fix
```javascript
db.adminCommand({ setDefaultRWConcern: 1, defaultWriteConcern: { w: 2, wtimeout: 10000 } });
db.adminCommand({ getDefaultRWConcern: 1 });
```

### For Verification During Incident
```javascript
rs.status().members.forEach(m => print(m.name, m.state, m.stateStr));
db.adminCommand({ getDefaultRWConcern: 1 });
db.test.insertOne({ test: new Date() });
```

### For Full Health Check
```javascript
db.serverStatus().repl;
db.adminCommand({ replSetGetStatus: 1 });
db.stats();
```

---

**Quick Reference Version:** 1.0  
**Last Updated:** 2026-06-16  
**For Emergency Access:** Yes - Can be executed by on-call DBA
