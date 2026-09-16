## Quality Attribute Scenarios

Non-functional requirements, expressed as six-part scenarios (*source, stimulus,
artifact, environment, response, response measure*). Each scenario is testable:
the response measure is the acceptance criterion.

The reference deployment for every scenario is a **single self-hosted node with
4 vCPU and 8 GB RAM**, serving a workspace of **50 members, 10 000 pages and
1 000 000 blocks**, unless a scenario states otherwise.

### Performance

**QA-01 — Editor input latency** *(constrains US-08, US-09)*

| Part | Value |
|---|---|
| Source | User typing in the editor |
| Stimulus | Inserts a character into a block |
| Artifact | Editor client and block persistence path |
| Environment | Normal operation, page containing 500 blocks |
| Response | Character is rendered locally and durably persisted |
| Response measure | Local render ≤ 50 ms (p95); server acknowledgement ≤ 500 ms (p95) |

**QA-02 — Real-time propagation latency** *(constrains US-12)*

| Part | Value |
|---|---|
| Source | Remote collaborator |
| Stimulus | Edits a block on a page open in other clients |
| Artifact | Synchronization channel |
| Environment | Normal operation, 5 concurrent editors on the same page |
| Response | Change is applied and rendered in every other connected client |
| Response measure | ≤ 300 ms (p95), ≤ 1 s (p99) from acknowledgement to remote render |

**QA-03 — Search response time** *(constrains US-14)*

| Part | Value |
|---|---|
| Source | User |
| Stimulus | Submits a full-text query over the workspace |
| Artifact | Search index |
| Environment | Normal operation, reference workspace size |
| Response | Ranked results are returned, scoped to the user's permissions |
| Response measure | ≤ 500 ms (p95) for the first page of results |

**QA-04 — Page open time** *(constrains US-04)*

| Part | Value |
|---|---|
| Source | User |
| Stimulus | Opens a page from the sidebar |
| Artifact | Page loading and rendering path |
| Environment | Cold client cache, page containing 500 blocks |
| Response | Page is rendered and accepts input |
| Response measure | Interactive within 1.5 s (p95) |

### Availability

**QA-05 — Process crash without data loss** *(constrains US-04, US-08)*

| Part | Value |
|---|---|
| Source | Infrastructure fault |
| Stimulus | The server process terminates abnormally |
| Artifact | Persistence layer |
| Environment | Normal operation, edits in flight |
| Response | The service restarts; every acknowledged edit survives |
| Response measure | Zero loss of acknowledged edits; service available again within 60 s |

**QA-06 — Editing during network interruption** *(constrains US-08, US-12)*

| Part | Value |
|---|---|
| Source | Network |
| Stimulus | The client loses connectivity for up to 5 minutes while the user is editing |
| Artifact | Editor client and local buffer |
| Environment | Degraded mode |
| Response | The editor stays usable, buffers edits locally, and reconciles them on reconnect without prompting the user to resolve conflicts |
| Response measure | 100 % of buffered edits are applied; reconnection completes within 10 s of link restoration |

### Data Consistency

**QA-07 — Concurrent edit convergence** *(constrains US-12)*

| Part | Value |
|---|---|
| Source | Multiple collaborators |
| Stimulus | 10 users edit the same block simultaneously |
| Artifact | Synchronization engine |
| Environment | Normal operation, client-server round-trip up to 500 ms |
| Response | All replicas converge to an identical document state; no acknowledged edit is silently discarded |
| Response measure | 100 % convergence within 2 s of the last edit, verified by an automated test over 1 000 randomized operation interleavings |

### Security

**QA-08 — Unauthorized page access** *(constrains US-11)*

| Part | Value |
|---|---|
| Source | Authenticated user without permission on the target page |
| Stimulus | Requests the page directly by identifier, bypassing the UI |
| Artifact | Authorization layer |
| Environment | Normal operation |
| Response | The request is denied and recorded in the audit log |
| Response measure | 100 % of such requests denied; responses for "forbidden" and "non-existent" are indistinguishable, leaking no title or metadata; audit entry written within 1 s |

**QA-09 — Credential protection** *(constrains US-01)*

| Part | Value |
|---|---|
| Source | Attacker |
| Stimulus | Obtains a database dump, or attempts repeated logins against one account |
| Artifact | Authentication subsystem |
| Environment | Normal operation |
| Response | Stored credentials are unusable; repeated attempts are throttled |
| Response measure | No password stored in plaintext or reversibly (Argon2id, per-user salt); more than 5 failed attempts per account per 15 minutes triggers rate limiting |
