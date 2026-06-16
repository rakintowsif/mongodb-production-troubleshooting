# Actual Cluster Status Output Reference
# Actual Cluster Status (2026-06-07)

## Replica Set Status

```javascript
mongodb-replica [direct: secondary] admin> rs.status()
{
  set: 'mongodb-replica',
  date: ISODate('2026-06-07T06:38:15.460Z'),
  myState: 2,
  term: Long('61'),
  syncSourceHost: 'mongodb1:27017',
  syncSourceId: 6,
  heartbeatIntervalMillis: Long('2000'),
  majorityVoteCount: 3,
  writeMajorityCount: 3,
  votingMembersCount: 5,
  writableVotingMembersCount: 4,
  optimes: {
    lastCommittedOpTime: { ts: Timestamp({ t: 1780814295, i: 1 }), t: Long('61') },
    lastCommittedWallTime: ISODate('2026-06-07T06:38:15.304Z'),
    readConcernMajorityOpTime: { ts: Timestamp({ t: 1780814295, i: 1 }), t: Long('61') },
    appliedOpTime: { ts: Timestamp({ t: 1780814295, i: 1 }), t: Long('61') },
    durableOpTime: { ts: Timestamp({ t: 1780814295, i: 1 }), t: Long('61') },
    lastAppliedWallTime: ISODate('2026-06-07T06:38:15.304Z'),
    lastDurableWallTime: ISODate('2026-06-07T06:38:15.304Z')
  },
  lastStableRecoveryTimestamp: Timestamp({ t: 1780814270, i: 1 }),
  electionParticipantMetrics: {
    votedForCandidate: true,
    electionTerm: Long('61'),
    lastVoteDate: ISODate('2026-04-01T02:11:01.125Z'),
    electionCandidateMemberId: 6,
    voteReason: '',
    lastAppliedOpTimeAtElection: { ts: Timestamp({ t: 1775009449, i: 1 }), t: Long('60') },
    maxAppliedOpTimeInSet: { ts: Timestamp({ t: 1775009449, i: 1 }), t: Long('60') },
    priorityAtElection: 15,
    newTermStartDate: ISODate('2026-04-01T02:11:05.124Z'),
    newTermAppliedDate: ISODate('2026-04-01T02:11:05.494Z')
  },
  members: [
    {
      _id: 4,
      name: 'mongodb5:27017',
      health: 1,
      state: 2,
      stateStr: 'SECONDARY',
      uptime: 184291,
      optime: { ts: Timestamp({ t: 1780814293, i: 1 }), t: Long('61') },
      optimeDurable: { ts: Timestamp({ t: 1780814293, i: 1 }), t: Long('61') },
      optimeDate: ISODate('2026-06-07T06:38:13.000Z'),
      optimeDurableDate: ISODate('2026-06-07T06:38:13.000Z'),
      lastAppliedWallTime: ISODate('2026-06-07T06:38:15.304Z'),
      lastDurableWallTime: ISODate('2026-06-07T06:38:15.304Z'),
      lastHeartbeat: ISODate('2026-06-07T06:38:14.387Z'),
      lastHeartbeatRecv: ISODate('2026-06-07T06:38:13.923Z'),
      pingMs: Long('2'),
      lastHeartbeatMessage: '',
      syncSourceHost: 'mongodb2:27017',
      syncSourceId: 7,
      infoMessage: '',
      configVersion: 42,
      configTerm: 61
    },
    {
      _id: 5,
      name: 'mongodb4:27017',
      health: 1,
      state: 2,
      stateStr: 'SECONDARY',
      uptime: 4720,
      optime: { ts: Timestamp({ t: 1780814293, i: 1 }), t: Long('61') },
      optimeDurable: { ts: Timestamp({ t: 1780814293, i: 1 }), t: Long('61') },
      optimeDate: ISODate('2026-06-07T06:38:13.000Z'),
      optimeDurableDate: ISODate('2026-06-07T06:38:13.000Z'),
      lastAppliedWallTime: ISODate('2026-06-07T06:38:15.304Z'),
      lastDurableWallTime: ISODate('2026-06-07T06:38:15.304Z'),
      lastHeartbeat: ISODate('2026-06-07T06:38:14.149Z'),
      lastHeartbeatRecv: ISODate('2026-06-07T06:38:14.933Z'),
      pingMs: Long('2'),
      lastHeartbeatMessage: '',
      syncSourceHost: 'mongodb5:27017',
      syncSourceId: 4,
      infoMessage: '',
      configVersion: 42,
      configTerm: 61
    },
    {
      _id: 6,
      name: 'mongodb1:27017',
      health: 1,
      state: 1,
      stateStr: 'PRIMARY',
      uptime: 5804854,
      optime: { ts: Timestamp({ t: 1780814293, i: 1 }), t: Long('61') },
      optimeDurable: { ts: Timestamp({ t: 1780814293, i: 1 }), t: Long('61') },
      optimeDate: ISODate('2026-06-07T06:38:13.000Z'),
      optimeDurableDate: ISODate('2026-06-07T06:38:13.000Z'),
      lastAppliedWallTime: ISODate('2026-06-07T06:38:13.427Z'),
      lastDurableWallTime: ISODate('2026-06-07T06:38:13.427Z'),
      lastHeartbeat: ISODate('2026-06-07T06:38:14.855Z'),
      lastHeartbeatRecv: ISODate('2026-06-07T06:38:15.113Z'),
      pingMs: Long('0'),
      lastHeartbeatMessage: '',
      syncSourceHost: '',
      syncSourceId: -1,
      infoMessage: '',
      electionTime: Timestamp({ t: 1775009461, i: 1 }),
      electionDate: ISODate('2026-04-01T02:11:01.000Z'),
      configVersion: 42,
      configTerm: 61
    },
    {
      _id: 7,
      name: 'mongodb2:27017',
      health: 1,
      state: 2,
      stateStr: 'SECONDARY',
      uptime: 20982395,
      optime: { ts: Timestamp({ t: 1780814295, i: 1 }), t: Long('61') },
      optimeDate: ISODate('2026-06-07T06:38:15.000Z'),
      lastAppliedWallTime: ISODate('2026-06-07T06:38:15.304Z'),
      lastDurableWallTime: ISODate('2026-06-07T06:38:15.304Z'),
      syncSourceHost: 'mongodb1:27017',
      syncSourceId: 6,
      infoMessage: '',
      configVersion: 42,
      configTerm: 61,
      self: true,
      lastHeartbeatMessage: ''
    },
    {
      _id: 8,
      name: 'mongodb9:27017',
      health: 1,
      state: 7,
      stateStr: 'ARBITER',
      uptime: 383072,
      lastHeartbeat: ISODate('2026-06-07T06:38:15.163Z'),
      lastHeartbeatRecv: ISODate('2026-06-07T06:38:15.326Z'),
      pingMs: Long('92'),
      lastHeartbeatMessage: '',
      syncSourceHost: '',
      syncSourceId: -1,
      infoMessage: '',
      configVersion: 42,
      configTerm: 61
    }
  ],
  ok: 1,
  '$clusterTime': {
    clusterTime: Timestamp({ t: 1780814295, i: 1 }),
    signature: {
      hash: Binary.createFromBase64('Sp94XXviqUsO6MM705rK1/rLVb0=', 0),
      keyId: Long('7609792762648461314')
    }
  },
  operationTime: Timestamp({ t: 1780814295, i: 1 })
}
}
```

## Write Concern Configuration

```javascript
mongodb-replica [direct: secondary] admin> db.adminCommand({ getDefaultRWConcern: 1 })
{
  defaultReadConcern: { level: 'local' },
  defaultWriteConcern: { w: 'majority', wtimeout: 0 },
  defaultWriteConcernSource: 'implicit',
  defaultReadConcernSource: 'implicit',
  localUpdateWallClockTime: ISODate('2026-06-07T06:44:24.598Z'),
  ok: 1,
  '$clusterTime': {
    clusterTime: Timestamp({ t: 1780814660, i: 1 }),
    signature: {
      hash: Binary.createFromBase64('HzruRC/FaZNbB511qigVAgV+xqk=', 0),
      keyId: Long('7609792762648461314')
    }
  },
  operationTime: Timestamp({ t: 1780814660, i: 1 })
}
}
```

## Key Facts
- **Cluster:** mongodb-replica (config v42, term 61)
- **Members:** 5 (4 data + 1 arbiter)
- **Issue:** `w: "majority"` needs 3 nodes, only 2 available when DR down
- **Replication Lag:** Minimal (all nodes in sync)
- **Status:** Healthy, demonstrating the exact problem scenario

**Captured:** 2026-06-07 06:38:15 UTC
