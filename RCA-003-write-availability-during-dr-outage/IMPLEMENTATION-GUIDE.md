# MongoDB Write Concern Configuration - Implementation Guide
# Implementation Guide

## Steps

### 1. Connect to Primary
```bash
mongosh admin -u admin -p<password>
```

### 2. Verify Current Config
```javascript
db.adminCommand({ getDefaultRWConcern: 1 })
```

### 3. Apply Fix
Execute this command to set the new default write concern:

```javascript
// Apply the permanent write concern fix
db.adminCommand({
  setDefaultRWConcern: 1,
  defaultWriteConcern: { 
    w: 2, 
    wtimeout: 10000 
  }
});
```

### Step 4: Verify the Fix
### 4. Verify Fix Applied
```javascript
db.adminCommand({ getDefaultRWConcern: 1 })
```
Expected: `"w": 2` and `"defaultWriteConcernSource": "global"`

### 5. Test Write
```javascript
db.test.insertOne({ test: "write_test", timestamp: new Date() })
```

---

## Rollback (if needed)
```javascript
db.adminCommand({
  setDefaultRWConcern: 1,
  defaultWriteConcern: { w: "majority", wtimeout: 0 }
});
```

---

## Monitoring
```javascript
rs.status()  // Check replication health
db.adminCommand({ replSetGetStatus: 1 })  // Full status
```

Monitor logs for: `write operation timed out waiting for replication` (indicates replication lag > 10s)

## Troubleshooting
| Issue | Cause | Solution |
|-------|-------|----------|
| Write timeouts | Replication lag > 10s | Check network, disk I/O |
| Lag increasing | Secondary slow | Check disk performance |
| Connection fails | Auth/network issue | Verify credentials, firewall |

---

**Document Version:** 1.0  
**Last Modified:** 2026-06-16  
**Created By:** DevOps Team  
**Verified On:** MongoDB 5.0, 6.0, 7.0
