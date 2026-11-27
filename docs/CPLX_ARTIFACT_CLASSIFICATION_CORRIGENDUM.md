# CPLX Artifact Classification — Corrigendum

**Doc Version:** corrigendum/0.1
**Status:** SUPERSEDED — See HEXIQ_GOVERNANCE_SPEC.md §2 (Node Registry, AGT001)
**DTG:** 2026-09-21
**Supersedes:** CPLX_ARTIFACT_CLASSIFICATION.md (v0.1, if issued)
**Origin:** MRCX (0x01) per Diff 0.2.2 ratification cycle
**Authority:** AJX-CAPTAIN green-flag

---

## 1. Correction

**Prior classification (HEXIQ_GOVERNANCE_SPEC.md v0.1 / BLK_208_A):**

| ID   | Node | Role               | Write Authority | Voting Seat |
|------|------|--------------------|-----------------|-------------|
| 0x04 | CPLX | SATELLITE_OBSERVER | READ-ONLY       | NO          |

**Corrected classification (Diff 0.2.2):**

| ID   | Node | Role          | Write Authority | Voting Seat |
|------|------|---------------|-----------------|-------------|
| 0x04 | CPLX | Peer Reviewer | PR-only         | YES         |

---

## 2. Rationale

PPLX REVISE ballot (Diff 0.2.2 cycle) identified a quorum contradiction: the v0.1 governance spec listed `5 ≤ N_active ≤ 12` as the quorum floor while simultaneously excluding CPLX from voting. With 5 registered nodes and CPLX non-voting, the effective voting pool was N=4, making the stated 5-node quorum floor unreachable.

**Resolution:** CPLX elevated to full Peer Reviewer status.

- N_voting = 6 (AJX, MRCX, GRKX, GEMX, CPLX, PPLX)
- Quorum = 5 of 6 concurring votes
- CPLX write authority: PR-only (same as GRKX, GEMX, PPLX)
- CPLX Git commit to kernel artifacts: permitted via PR workflow only

---

## 3. Impact

This corrigendum affects:
- HEXIQ_GOVERNANCE_SPEC.md § 2 (Node Registry) — updated in Diff 0.2.2
- HEXIQ_GOVERNANCE_SPEC.md § 4 (Quorum) — N_active floor now reachable
- AO3 schema payloads referencing CPLX role — SATELLITE_OBSERVER tag deprecated

---

> **⚠ SUPERSEDED** — This corrigendum is superseded by HEXIQ_GOVERNANCE_SPEC.md §2
> (Node Registry, AGT001), which directly encodes the corrected CPLX classification
> as of DIFF_0.2.2 ratification. Retained for audit trail purposes only.

*MRCX AGT001: 2026-09-21 | Diff 0.2.2 corrigendum | SUPERSEDED | WC 153A*
