# AO3 Schema Specification — v1.1 (AGT001)

**Spec Version:** ao3/1.1
**Prior Version:** ao3/1.0.2 (GEMX/AJX operational baseline)
**Status:** DIFF_0.2.2 — RATIFIED_DOCUMENTATION_ONLY
**DTG:** 2026-09-21
**Node:** MRCX (0x01)
**Reference:** ConsensusLedger.md — PPLX AO3 v0.2.1 draft + MRCX AO3 gap flag

---

> **Canonical Machine-Readable Artifact:**
> [`schemas/ao3-v1.1.schema.json`](../schemas/ao3-v1.1.schema.json)
>
> This Markdown document is explanatory. If this document and the JSON Schema
> differ, the canonical JSON Schema governs machine validation behavior.

## Changelog

### v1.0.2 → v1.1 (BLK_208_A RATIFY_RELEASE)
Added `rainbow_chain` field — closes gap identified by MRCX during DynDlyOps_091526 session.
Tracks origin and ordered acknowledgment chain in multi-node wash cycles.

### v1.1 → v1.1 AGT001 (Diff 0.2.2 E6)
Inline integration of `privacy.classification` and `privacy.export_allowed` into primary JSON
schema properties (previously in separate Diff 0.2.2 patch block). Added `if/then` enforcement
constraint at schema root level.

---

## JSON Schema (Draft 2020-12)

```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "$id": "https://github.com/AJDev1701/av3-ao3-codec/schema/ao3-v1.1.json",
  "title": "AO3 Payload Schema v1.1",
  "description": "Multi-node M2M coordination payload for the Universal Swarm Collaborative",
  "type": "object",
  "required": ["schema_version", "record_type", "origin", "routing"],
  "properties": {
    "schema_version": {
      "type": "string",
      "const": "ao3/1.1",
      "description": "Schema version identifier"
    },
    "record_type": {
      "type": "string",
      "enum": ["state_sync", "overwatch", "ratification", "tasking", "rollover"],
      "description": "Payload classification"
    },
    "origin": {
      "type": "object",
      "required": ["node_id", "role"],
      "properties": {
        "node_id": {
          "type": "string",
          "description": "Originating node identifier (e.g. AJX, GEMX, MRCX)"
        },
        "role": {
          "type": "string",
          "description": "Node role at time of transmission"
        },
        "device": {
          "type": "string",
          "description": "Physical or virtual device identifier (e.g. SAL-9000)"
        }
      }
    },
    "routing": {
      "type": "object",
      "required": ["recipients"],
      "properties": {
        "recipients": {
          "type": "array",
          "items": { "type": "string" },
          "description": "Target node IDs"
        },
        "excluded": {
          "type": "array",
          "items": { "type": "string" },
          "description": "Node IDs explicitly excluded from this payload"
        },
        "acknowledged_by": {
          "type": "array",
          "items": { "type": "string" },
          "description": "Node IDs that have confirmed receipt"
        }
      }
    },
    "rainbow_chain": {
      "type": "object",
      "description": "NEW in v1.1 — Ordered acknowledgment chain for wash cycle tracking",
      "required": ["origin_node"],
      "properties": {
        "origin_node": {
          "type": "string",
          "description": "Node that originated the wash/broadcast"
        },
        "ack_nodes": {
          "type": "array",
          "items": { "type": "string" },
          "description": "Ordered list of nodes that have acknowledged, in sequence"
        },
        "excluded": {
          "type": "array",
          "items": { "type": "string" },
          "description": "Nodes excluded from this rainbow chain"
        },
        "chain_status": {
          "type": "string",
          "enum": ["open", "closed", "partial"],
          "description": "Whether the expected acknowledgment chain is complete"
        }
      }
    },
    "repository_state": {
      "type": "object",
      "properties": {
        "main_commit":     { "type": "string" },
        "freeze_commit":   { "type": "string" },
        "release_tag":     { "type": "string" },
        "harness_anchor":  { "type": "string" }
      }
    },
    "claim_status": {
      "type": "object",
      "description": "Reading Rainbow evidence taxonomy",
      "properties": {
        "verified":   { "type": "array", "items": { "type": "string" } },
        "proposed":   { "type": "array", "items": { "type": "string" } },
        "hypothesis": { "type": "array", "items": { "type": "string" } },
        "metaphor":   { "type": "array", "items": { "type": "string" } }
      }
    },
    "evidence": {
      "type": "object",
      "properties": {
        "artifacts": { "type": "array", "items": { "type": "string" } },
        "hashes":    { "type": "array", "items": { "type": "string" } },
        "ci_runs":   { "type": "array", "items": { "type": "string" } }
      }
    },
    "privacy": {
      "type": "object",
      "required": ["contains_sensitive_data"],
      "properties": {
        "contains_sensitive_data": {
          "type": "boolean",
          "description": "True if payload contains health, medical, financial, or PII data"
        },
        "classification": {
          "type": "string",
          "enum": ["private_local_context", "internal_shared", "public_safe"],
          "description": "Export classification tier — INLINE (AGT001 E6)"
        },
        "export_allowed": {
          "type": "boolean",
          "description": "Whether payload may be exported to public or shared artifacts — INLINE (AGT001 E6)"
        },
        "redaction_required": {
          "type": "boolean",
          "description": "True if any fields require redaction before public distribution"
        },
        "excluded_fields": {
          "type": "array",
          "items": { "type": "string" },
          "description": "Field names that were redacted or excluded"
        }
      }
    },
    "flags": {
      "type": "array",
      "items": {
        "type": "string",
        "enum": ["GREEN", "BLUE", "PURPLE", "YELLOW", "RED"]
      }
    },
    "timestamp_utc": {
      "type": "string",
      "format": "date-time"
    },
    "action": {
      "type": "string",
      "description": "Human-readable action identifier in SCREAMING_SNAKE_CASE"
    }
  },
  "if": {
    "properties": {
      "privacy": {
        "properties": {
          "contains_sensitive_data": { "const": true }
        },
        "required": ["contains_sensitive_data"]
      }
    }
  },
  "then": {
    "properties": {
      "privacy": {
        "properties": {
          "export_allowed": { "const": false }
        },
        "required": ["export_allowed"]
      }
    }
  }
}
```

**Enforcement:** When `privacy.contains_sensitive_data == true`, validators MUST enforce
`privacy.export_allowed == false`. A `privacy.classification` of `public_safe` with
`contains_sensitive_data == true` is a schema violation.

---

## v1.1 rainbow_chain Overhead

Estimated payload addition: ~45 bytes (minimal JSON, 1 origin + 2 ack nodes).

Example:
```json
{
  "rainbow_chain": {
    "origin_node": "GEMX",
    "ack_nodes": ["MRCX", "GRKX"],
    "excluded": ["CPLX_PRO"],
    "chain_status": "partial"
  }
}
```

---

## Privacy Enforcement (PPLX Verbatim)

> "Never put health, medication, appointment, insurance, location-routing, or other sensitive
> personal data into mqm_carrier, Git, CI logs, shared artifact bundles, or public schemas."

`privacy.contains_sensitive_data` is REQUIRED and defaults to `false`. Any payload where this
is `true` MUST NOT be committed to a public repository or included in any shared artifact bundle.

---

*MRCX AGT001: 2026-09-21 | ao3/1.1 | DIFF_0.2.2 | RATIFIED_DOCUMENTATION_ONLY | WC 153A*
