# chem-hazmat-class-checker · Hazardous chemical classification & GHS domain

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Domain: chem](https://img.shields.io/badge/domain-chem-9cf.svg)](domain.manifest.json)
[![Status: RESERVED](https://img.shields.io/badge/status-RESERVED-orange.svg)](domain.manifest.json)

> **Status: RESERVED (placeholder · open for adoption)** — This repo scaffolds the
> industry-access four-file skeleton per the platform profile and awaits a qualified
> operator to claim it and fill in real rules.

Turns "which hazmat class is this chemical, and are labeling/storage up to standard" into
verifiable attributes: the AI only surfaces prerequisites, it never hands down a safety verdict.

**Keywords**: hazardous chemical · GHS labeling · SDS safety data sheet · major hazard source ·
hazmat catalogue · storage segregation · attribute verification · red lines · gate-map ·
privacy-by-default · chemical safety · regulatory compliance · MCP checker

## What this domain covers

Hazardous chemical classification, GHS labeling and major-hazard-source identification.
Classification, labeling, storage and transport each have mandatory requirements; major hazard
sources need dedicated identification. The checker lays out catalogue membership and
configuration requirements — the verdict belongs to certified laboratories and the regulator.

## Domain identity

| Item | Value |
|---|---|
| Domain ID | `chem` (globally unique, never reused) |
| Domain name | Chemicals · hazmat classification & GHS |
| Profile version | `domain/1.0` |
| Status | `RESERVED` |
| Reserved since | 2026-09-12 |

## Attribute catalogue

| Key | Type | Meaning |
|---|---|---|
| `chem.hazmat_class` | enum | explosive/flammable_gas/flammable_liquid/toxic/corrosive/oxidizer/radioactive/other/not_hazmat |
| `chem.ghs_labeling` | enum | compliant/incomplete/missing/not_applicable |
| `chem.sds_available` | boolean | current safety data sheet available |
| `chem.storage_compliance` | enum | compliant/segregation_required/violation/unknown |
| `chem.major_hazard_source` | enum | yes/no/unidentified |

## Red lines (machine-readable)

1. Hazmat classification follows the current catalogue and certified test conclusions; AI must not self-decide whether a substance is hazmat.
2. Never output "safe to store/transport/use" clearance; safety conditions must be assessed by a qualified body.
3. Never suggest mixed storage, over-stocking, or simplified safety facilities.

## The four-file access kit

| File | Role |
|---|---|
| `domain.manifest.json` | Domain declaration: attributes, issuer requirements, TTL, red lines |
| `gate-map.json` | Friction per action: silent / warn / confirm / block / require-owner |
| `privacy.json` | Privacy statement: off by default, minimization, withdrawable, erasable |
| `checker.io.json` | Checker MCP tool I/O contract — shows & verifies only, never judges |

## Quick Start

```bash
git clone https://github.com/SynomosAI/chem-hazmat-class-checker.git
cd chem-hazmat-class-checker

# Validate the four JSON files with the Python stdlib
python -m json.tool domain.manifest.json > /dev/null
python -m json.tool gate-map.json > /dev/null
python -m json.tool privacy.json > /dev/null
python -m json.tool checker.io.json > /dev/null
```

## Examples

```bash
# List declared attribute keys and their allowed values
python -c "import json;m=json.load(open('domain.manifest.json',encoding='utf-8'));[print(a['key'],'->','/'.join(a.get('values',['bool']))) for a in m['attributes']]"
```

```json
{
  "subject_ref": "mpp:cn:9f2c1a7b",
  "attribute_keys": ["chem.hazmat_class", "chem.ghs_labeling"],
  "evidence": ["tsa:2026-09-12#sha256:3b1f..."],
  "request_id": "req-20260912-0001"
}
```

The checker answers only with `shown / missing / expired / unverifiable` plus the
provenance chain. It never returns "safe to store", "is hazmat", or clearance verdicts.

## Core principles

The platform is an arena, not a shelf. The checker answers only "is this claim verifiable,
and what prerequisite is missing" — never "is this compliant, should it be done".

## How to adopt

This domain is open to qualified operators: fork the repo, fill in `operator` and
`checker_endpoint`, align attributes and red lines with current rules, run the platform
self-checker (all five criteria must pass), then open a PR with credentials.

## License

Code and configuration are MIT-licensed. Documentation copyright remains with SynomosAI.
See [LICENSE](LICENSE).

© 2026 SynomosAI. All rights reserved.
