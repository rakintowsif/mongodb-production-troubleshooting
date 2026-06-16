# MongoDB Cluster Configuration Reference
## Members
| ID | Host | Role | DC | Priority |
|----|----|------|----|----|
| 6 | mongodb1:27017 | PRIMARY | Primary | 3 |
| 7 | mongodb2:27017 | SECONDARY | Primary | 2 |
| 5 | mongodb4:27017 | SECONDARY | DR | 1 |
| 4 | mongodb5:27017 | SECONDARY | DR | 0 |
| 8 | mongodb9:27017 | ARBITER | AWS | 0 |

## Write Concern Comparison
| State | w | wtimeout | Available | Status |
|------|---|----------|-----------|--------|
| Before Fix | "majority" | 0 | 2 nodes | ❌ Blocked |
| After Fix | 2 | 10000ms | 2 nodes | ✅ Operational |

## Network Latency
```
Primary ↔ Primary: < 1ms
Primary ↔ DR:      < 50ms  
Primary ↔ AWS:     < 100ms
```

## Monitoring
```
Replication lag: < 100ms (normal)
Write acks: 2 nodes
Heartbeat: 2000ms
```
