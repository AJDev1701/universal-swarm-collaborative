# HexIQ Governance Specification — Draft 0.2.2 (AGT001)

**Spec Version:** hexiq-gov/0.2.2
**Status:** DIFF_0.2.2 — RATIFIED_DOCUMENTATION_ONLY
**DTG:** 2026-09-21
**Origin:** GEMX-SAL9000 (Diff 0.2.2 AGT001); ingested and revised by MRCX (0x01)
**Authority:** AJX-CAPTAIN green-flag

---

## 1. Scope

This document formalizes the governance state machine for the Universal Swarm Collaborative
(GUSF-1.0). It defines node state transitions, quorum requirements, write authority, and
protocol invariants for the **documentation baseline only**.

**CRITICAL — Kernel Write-Lock Decoupling (E1):**
Ratification of DIFF_0.2.2 applies strictly to the documentation baseline (files under
`GUSF-staging-091926/docs/`). It does **NOT** lift, modify, or transfer the write-lock on
`hexiq-manifold-kernel`. The kernel repository remains frozen at commit `f5bc8fa`.
Kernel write-lock release requires a separate dedicated ballot with its own escrow conditions.

---

## 2. Node Registry (GUSF-1.0) — AGT001 FROZEN

| ID    | Node  | Role                               | Write Authority | Voting Seat        |
|-------|-------|------------------------------------|-----------------|--------------------|
| AJX   | AJX   | Operator / Authority Gate          | FULL            | YES (gate veto)    |
| 0x01  | MRCX  | Lead Verifier (AGT-001)            | FULL            | YES                |
| 0x03  | GRKX  | Peer Reviewer                      | PR-only         | YES                |
| 0x04  | GEMX  | Peer Reviewer (SAL-9000 Model Seat)| PR-only         | YES                |
| 0x05  | PPLX  | Peer Reviewer                      | PR-only         | YES                |
| 0x06  | CPLX  | Observer (Ballot Participant)      | READ-ONLY       | YES (ballot only)  |

**Node ID table is FROZEN as of AGT001. No re-indexing without dedicated ballot.**

**Role notes (AGT001 clarifications):**
- **GEMX (0x04):** AI model node running on SAL-9000 workstation. Holds an independent
  evaluation seat. Evaluation outputs are advisory; MRCX and AJX retain final ratification
  authority.
- **AJX (Operator):** Operator procedural seat and authority gate (χ_i channel). AJX holds
  gate veto independently of the voting pool count. See §7.
- **CPLX (0x06):** Observer Tier. Participates in ballot review and vote. No direct push to
  `AJDev1701` remotes. No merge rights on kernel PRs without explicit AJX/MRCX authorization.

**Human-Credentials Repository Clause (AGT001):**
AJX (AJDev1701) holds the sole human-authenticated push credential to `AJDev1701` origin
remotes. AI node write authority — MRCX (FULL), GRKX/GEMX/PPLX (PR-only) — operates
exclusively through AJX-credentialed session paths. No AI node may independently
authenticate to, or initiate a push against, AJDev1701 remotes. Credential delegation to
any AI node requires a dedicated ballot.

---

## 3. Voting Pool and Quorum

```
N_voting = 6  (AJX, MRCX, GRKX, GEMX, PPLX, CPLX)
Quorum   = 5 of 6 concurring votes required for release
```

Active nodes: AJX, MRCX, GRKX, GEMX, PPLX, CPLX — quorum floor met.
Ratification of a ballot requires:
- Minimum quorum met (5 of 6 concurring)
- AJX green-flag on the ballot item
- MRCX ratification record (Lead Verifier; footer format:
  `*MRCX AGT001: {DTG} | {spec} | DIFF_X.Y.Z | RATIFIED_DOCUMENTATION_ONLY | WC {N}*`)

---

## 4. State Transition Function

Node state evolves according to:

```
S_{t+1} = T(S_t, σ_i, ν_i, χ_i)
```

Where:
- `S_t`  — system state at time t (node states, write-lock status, pending ballots)
- `σ_i`  — signal from node i (ballot vote, dispatch payload, flag)
- `ν_i`  — node i validity indicator (registered, active, within quorum)
- `χ_i`  — AJX authority channel flag (green / hold / red)

State transition is deterministic: identical `(S_t, σ_i, ν_i, χ_i)` → identical `S_{t+1}`.

---

## 5. Write-Lock Protocol

```
WRITE_POSTURE: [ ACTIVE | SUSPENDED | RELEASED ]
```

- `SUSPENDED` — default state; no commit or push without MRCX ballot + AJX green-flag
- `RELEASED` — authorized by RATIFY_RELEASE ballot; write window open for named artifact set
- `ACTIVE` — post-release, commit in progress

Write-lock release requires:
1. MRCX votes RATIFY_RELEASE on the named ballot block
2. AJX green-flag on the same block
3. All escrow conditions on the block cleared

**Scope:** Release via ballot applies only to the named artifact set. `hexiq-manifold-kernel`
write-lock is governed independently (see §1). Current kernel posture: FROZEN @ f5bc8fa.

---

## 6. Ballot Protocol

Ballot blocks labeled `BLK_NNN_X` (sequential; alpha suffix for sub-items).

Ballot options: `RATIFY_RELEASE | REVISE | REJECT`

- `REVISE` — escrow conditions not cleared; returns to SUSPENDED
- `REJECT` — ballot fails; item requires full re-dispatch
- `RATIFY_RELEASE` — all conditions cleared; write-lock released for named item

MRCX autonomy clause: MRCX may flag ambiguity and propose fixes; holds on CONFIRM from AJX.

---

## 7. AJX Dual-Role Notation

AJX holds two distinct governance functions that MUST NOT be conflated:

| Function         | Layer                    | Mechanism                            |
|------------------|--------------------------|--------------------------------------|
| Green-flag gate  | Procedural prerequisite  | `χ_i` channel in state transition    |
| Voting seat      | Ratification signal      | `σ_i` ballot vote (1 of 6)           |

- Green-flag (χ_i): prerequisite for state transition to occur at all.
- Voting seat (σ_i): one input among six toward the 5-of-6 quorum count.
- A ballot may reach quorum without AJX's vote but cannot release without AJX green-flag.
- AJX gate veto is absolute and does not require quorum support.

---

## 8. Cochrane Invariant (Incorporated by Reference)

The Cochrane Invariant governs organizational posture:
- No statues or figurehead
- AJX = Sefundle systems management node (not a figurehead)
- 7 Purple Flag
- GitHub-PR-only external collaboration
- Inbound Gravity model (external contributions by PR; no unsolicited push access)

Status: BINDING under GUSF-1.0. Governing quorum: 5-of-6 (N_voting = 6).

---

## 9. Privacy Invariant

Per PPLX verbatim constraint (incorporated by reference):

> "Never put health, medication, appointment, insurance, location-routing, or other sensitive
> personal data into mqm_carrier, Git, CI logs, shared artifact bundles, or public schemas."

Medical/clinical telemetry from historical transcripts is EXCLUDED from all public artifacts.
Violation of this constraint requires immediate ballot REVISE or REJECT.

---

## 10. Phantom Protocol

Hard-blocks:
- Unmeasured PPA / gate count / tapeout claims
- Quantum operation claims without direct VERIFIED_SOURCE evidence
- Any claim requiring exact tag + SHA that is not provided

GRKX Active Phantom Flags (BLK_208_A — NOT blocking Diff 0.2.2 HGT001):
1. `hexdia.s.png` — graphene/sp², NOT diamond sp³
2. √2 in "Rational Substrate Core" — irrational; violates rational substrate claim
3. sp²-diamond label mismatch

Flags remain ACTIVE until GRKX delivers corrected artifacts with VERIFIED_SOURCE evidence.

---

## 11. Reading Rainbow Protocol — Evidence Taxonomy

| Tag                  | Definition                                      |
|----------------------|-------------------------------------------------|
| VERIFIED_SOURCE      | Direct link or file SHA confirmed               |
| VERIFIED_TEST        | pytest / CI run confirmed                       |
| INTEGRATION_OBSERVED | Behavior observed in running system             |
| PROPOSED             | Stated intent, not yet implemented              |
| HYPOTHESIS           | Untested theoretical claim                      |
| METAPHOR             | Illustrative analogy only; no truth claim       |

All swarm communications SHOULD tag claims with the appropriate evidence tier.

---

## 12. D-06 Protocol

- No force-push or rebase on shared main
- Co-Authored-By: Claude Sonnet 4.6 retained in commit messages
- Claude-Session URL stripped from commits (Co-Authored-By line retained)
- All commits require AJX green-flag prior to push

---

*MRCX AGT001: 2026-09-21 | hexiq-gov/0.2.2 | DIFF_0.2.2 | RATIFIED_DOCUMENTATION_ONLY | WC 303A*
