# ISSUE 1948: All Core Devs - Testing (ACDT) #72, March 2, 2026
Created: 2026-02-24T12:11:12Z by parithosh

### UTC Date & Time

[March 02, 2026, 14:00 UTC](https://savvytime.com/converter/utc/mar-2-2026/2pm)

### Agenda

Fusaka:
    - blob-devnet-0 updates
   
Glamsterdam:
    - bal-devnet-2 updates
    - bal-devnet-3 readiness
    - epbs-devnet-0 implementation update
        - client readiness check

Gas limit:
   - eth/70 implementation update

State bloat:
    - perf-devnet-3 updates

### Call Series

All Core Devs - Testing

### Autopilot Mode

- [x] Use autopilot (recommended defaults for this call series)


<details>
<summary>🔧 Meeting Configuration</summary>

### Duration

60 minutes

### Occurrence Rate

weekly

### Use Custom Meeting Link (Optional)

- [ ] I will provide my own meeting link

### Display Zoom Link in Calendar Invite (Optional)

- [x] Display Zoom link in invite

### YouTube Livestream Link (Optional)

- [x] Create YouTube livestream link
</details>



===== COMMENTS =====

--- COMMENT by github-actions[bot] at 2026-02-24T12:12:22Z ---
⚡ **Protocol Call Resources:**

✅ **Zoom**: [Join Meeting](https://ethereumfoundation.zoom.us/j/88479308162?pwd=9XvtF4kjIfZ42rQrvySQLJPu9bLz7u.1)
✅ **Calendar**: [Add to Calendar](https://www.google.com/calendar/event?eid=ZGw2YzM0NTNyaGhwM2IzNmthamMzY3BjM2MgY191cGFvZm9uZzhtZ3JtcmtlZ243aWM3aGs1c0Bn)
✅ **Discourse**: [Discussion Topic](https://ethereum-magicians.org/t/27817)
✅ **YouTube Live**: [Watch Live](https://youtube.com/watch?v=J0RwITMn2UM)

--- COMMENT by danceratopz at 2026-02-27T08:29:34Z ---
Excuse me for cross-posting this in multiple issues/channels! But in the hopes of avoiding disruption to client team CIs...

Just a quick head's up for client teams: Future excution-specs fixture releases will follow a different directory layout (fixture formats themselves haven't changed). Please read more in [announcment in the Eth R&D #el-testing channel](https://discord.com/channels/595666850260713488/753271902520213625/1476579508030013662).

But the TLDR is:
Previously, fixture JSON files accumulated test cases for every target fork in a single file, meaning each file grew with every new fork. Fixtures are now split into per-fork sub-directories, keeping file sizes bounded and letting you point your runner at exactly the fork you need to test.

And the layout is:
```
blockchain_tests_engine/
├── for_paris/
│   ├── paris/
│   │   ├── eip7610_create_collision/
│   │   ├── revert_in_create/
│   │   └── security/
│   └── shanghai/
│       └── eip3651_warm_coinbase/
└── for_shanghai/
    ├── paris/
    │   ├── eip7610_create_collision/
    │   └── ...
    └── shanghai/
        ├── eip3651_warm_coinbase/
        ├── eip3855_push0/
        ├── eip3860_initcode/
        └── eip4895_withdrawals/
```

--- COMMENT by yoitsdave0415-star at 2026-02-27T12:15:27Z ---
### 

--- COMMENT by yoitsdave0415-star at 2026-02-27T12:16:40Z ---
I need a blunt 

--- COMMENT by akashkshirsagar31 at 2026-03-02T07:59:03Z ---
X Stream: https://x.com/i/broadcasts/1OxwblnQZjNJB

--- COMMENT by jochem-brouwer at 2026-03-02T10:45:45Z ---
I would like to talk/discuss about testing/benchmarks on state and how to get to this state, especially big ones. How to init the chain? How can we improve the DevEx for people doing research on state? 😄 👍 

We are working on a tool to directly launch clients into this big state: https://github.com/nerolation/state-actor/tree/main and could use help there 😄 👍 
