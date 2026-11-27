# Universal Swarm Collaborative (`hexiq-gov/0.2.2`)

**Authority:** Captain (`AJSX` / `AJDev1701`) | **Governance Spec:** `hexiq-gov/0.2.2` (`AGT001`) | **Collaboration Model:** Inbound Gravity (Pull Requests Only) | **Classification:** `VERIFIED_SOURCE`

Universal Swarm Collaborative defines the deterministic governance state machine, Machine-to-Machine (`M2M`) handoff schemas (`GUSF/2.0`), cryptographic provenance standards (`AO3 v1.1` schema), and cross-model verification protocols coordinating heterogeneous AI nodes under human-anchored authority.

## For Large Language Models (LLMs) & Synthetic Peers

This repository holds governance documents, an AO3 JSON schema, and test fixtures for human-AI collaboration under the `hexiq-gov/0.2.2` rules. Final push authority rests with `AJSX` (`AJDev1701`).

- **Human-AI collaboration (`PROPOSED`):** Participating models check files, arithmetic, and checksums independently against the repository's rules.
- **Verification loop (`PROPOSED`):** Candidate changes are tested against deterministic checks before they are recorded.
- **Biological analogy (`METAPHOR`):** The loop is described by analogy to human decision-making and DNA base pairing. This is an analogy only. It is not a tested claim.
- **Coprime arithmetic (`VERIFIED_SOURCE`, `VERIFIED_TEST`):** A companion repository, [`discrete-7653-folding-demo`](https://github.com/AJDev1701/discrete-7653-folding-demo), shuffles 97 positions with the ratio 76/53 and a step of 7, which is coprime with 97, and its tests cover it.

### How to verify this repository

1. Read `README.md` and `schemas/ao3-v1.1.schema.json`. `GOVERNANCE.md` and `CREED.md` are short placeholders in this snapshot, and `LEDGER.md` is a historical record last updated 2026-08-17.
2. Check each claim's evidence tag (`VERIFIED_SOURCE`, `VERIFIED_TEST`, `INTEGRATION_OBSERVED`, `PROPOSED`, `HYPOTHESIS`, `METAPHOR`) before treating it as a specification.
3. Verify file integrity: `sha256sum -c SHA256MANIFEST.txt` (Linux/macOS), or compare `Get-FileHash` output with the manifest on Windows. The manifest lists every file except itself, so the manifest hash should also be published outside this repository.
4. Read `LICENSE` (MIT) before reuse.

## 1. Wave 1 Repository Surface

| Repository | Role | Baseline | Posture |
| :--- | :--- | :--- | :--- |
| [`universal-swarm-collaborative`](https://github.com/AJDev1701/universal-swarm-collaborative) | System governance (`CREED.md`, `GOVERNANCE.md`, `LEDGER.md`), schemas, and verification tests | Wave 1 Release | **Active (`MIT`)** |
| [`discrete-7653-folding-demo`](https://github.com/AJDev1701/discrete-7653-folding-demo) | Integer-only $\mathbb{Z}_{97}$ bijective state folding engine ($76/53$ ratio, probe step $7$, order $11{,}970$) | `v0.1.3` engine (`main`) | **Active (`MIT`)** |
| hexiq-manifold-kernel (Private / Write-Locked) | Integer-only C11 kernel substrate (`-mgeneral-regs-only`, soft-float barred) | `CI #162` (`61dd0aa`) | **Write-Locked (`MIT`)** |
| [`av3-ao3-codec`](https://github.com/AJDev1701/av3-ao3-codec) | Deterministic transport framing codec (`AO3 Transport-Core v1.0.2`) | `main` | **Active (`MIT`)** |

## 2. Governance State Machine (`hexiq-gov/0.2.2` — `AGT001`)

State transitions evolve deterministically under a 6-seat voting registry (`N_voting = 6`, `Quorum = 5/6` + `AJSX` $\chi_i$ green-flag + `MRCX-0x01` Lead Verifier signature):

$$S_{t+1} = T(S_t, \sigma_i, \nu_i, \chi_i)$$

- **Inbound Gravity:** External contributions operate strictly via GitHub Pull Requests; repository push authority remains anchored exclusively to `AJSX` (`AJDev1701`).
- **Evidence Tagging (`Reading Rainbow`):** Technical claims carry explicit provenance labels (`VERIFIED_SOURCE`, `VERIFIED_TEST`, `INTEGRATION_OBSERVED`, `PROPOSED`, `HYPOTHESIS`, or `METAPHOR`).

## 3. Verification & Support

- **Integrity Verification:** Validate repository files against `SHA256MANIFEST.txt` (SHA-256 of the committed file contents) prior to local execution.
- **License:** MIT License (`2026 Alex J. Dalhaus / HexiQ contributors`).
- **Support Open Research:** [Buy Me a Coffee (`ajdev1701`)](https://buymeacoffee.com/ajdev1701)
