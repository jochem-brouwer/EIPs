# ISSUE 1878: All Core Devs - Testing (ACDT) #66, January 19, 2026
Created: 2026-01-15T14:48:01Z by parithosh

### UTC Date & Time

[January 19, 2026, 14:00 UTC](https://savvytime.com/converter/utc/jan-19-2026/2pm)

### Agenda

Fusaka:
    - Mainnet BPO outcome

Glamsterdam:
    - bal-devnet-0/1 updates and scope discussion
    - epbs-devnet-0 update

EIP-8037 decision for glamsterdam


    


### Call Series

All Core Devs - Testing


<details>
<summary>🔧 Meeting Configuration</summary>

### Duration

60 minutes

### Occurrence Rate

weekly

### Use Custom Meeting Link (Optional)

- [ ] I will provide my own meeting link

### Facilitator Emails (Optional)

parithosh@ethereum.org

### Display Zoom Link in Calendar Invite (Optional)

- [ ] Display Zoom link in invite

### YouTube Livestream Link (Optional)

- [x] Create YouTube livestream link
</details>



===== COMMENTS =====

--- COMMENT by github-actions[bot] at 2026-01-15T14:49:08Z ---
⚡ **Protocol Call Resources:**

✅ **Zoom**: [Join Meeting](https://ethereumfoundation.zoom.us/j/88479308162?pwd=9XvtF4kjIfZ42rQrvySQLJPu9bLz7u.1)
✅ **Calendar**: [Add to Calendar](https://www.google.com/calendar/event?eid=ZGw2YzM0NTNyaGhwM2IzNmthamMzY3BjM2MgY191cGFvZm9uZzhtZ3JtcmtlZ243aWM3aGs1c0Bn)
✅ **Discourse**: [Discussion Topic](https://ethereum-magicians.org/t/27395)
✅ **YouTube Live**: [Watch Live](https://youtube.com/watch?v=Y61OpUvVpFM)

--- COMMENT by abcoathup at 2026-01-16T08:31:06Z ---
Forkcast devnet tracker https://forkcast.org/devnets/

--- COMMENT by spencer-tb at 2026-01-16T11:02:05Z ---
#### Finalize `bal-devnet-2` Scope

Proposing we keep EIP-8024, EIP-7778 & EIP-7708 and remove EIP-7843, more below:
- **_EIP-7843_** - Agree to push to devnet-3 or align on keeping it in devnet-2. Requires CL changes (small). Currently underspecified, PRs to address the latter:
  - https://github.com/ethereum/EIPs/pull/11083/
  - https://github.com/ethereum/execution-apis/pull/731
- **_EIP-8024_**  - Approve and merge spec changes following alignment in ACD: https://github.com/ethereum/EIPs/pull/11094
- **_EIP-7778_** - Confirm alignment on spec (@nerolation comment below): https://github.com/ethereum/execution-specs/pull/1401#issuecomment-3759373893
- **_EIP-7708_** - Approve and merge spec changes: https://github.com/ethereum/EIPs/pull/9003


EELS PRs for visability:
- https://github.com/ethereum/execution-specs/pull/2021
- https://github.com/ethereum/execution-specs/pull/1401
- https://github.com/ethereum/execution-specs/pull/2026
- https://github.com/ethereum/execution-specs/pull/2023

--- COMMENT by nerolation at 2026-01-17T10:18:54Z ---
Regarding EIP-7778, I'd like to quickly discuss if we want to go with the gas used before or after refunds. For context, please check out this discord message:
https://discord.com/channels/595666850260713488/688075293562503241/1461678093289656524

Here's the related PR against the execution-specs:
https://github.com/ethereum/execution-specs/pull/1401

--- COMMENT by akashkshirsagar31 at 2026-01-19T07:49:36Z ---
X Stream: https://x.com/i/broadcasts/1mrxmBazQvwKy

--- COMMENT by bharath-123 at 2026-01-19T11:57:35Z ---
I would like to discuss absorbing MEV-Boost into clients. I have written a simple specification on how we could absorb MEV-Boost into clients https://github.com/ethereum/builder-specs/pull/144 . I would like to get a temperature check from clients on whether we want to do this in Glamsterdam or not.
