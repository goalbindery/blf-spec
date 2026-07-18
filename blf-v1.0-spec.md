# Bindery Learning Format (BLF) — v1.0 (draft)

**Status:** Draft, formalized from GoalBindery Proposal v0.4, Appendix B
**Format name:** Bindery Learning Format (BLF) — retained as the format name under the GoalBindery brand
**Canonical schema (reserved):** `https://goalbindery.app/schemas/blf/v1.json`
**License:** Creative Commons Attribution 4.0 (CC-BY-4.0); schema itself in the public domain
**Intended home:** the public `goalbindery/blf-spec` repository (promote after review)

---

## Overview

BLF is a JSON-based open specification for portable Learning Book content. Every GoalBindery installation can produce and consume BLF. The format is published under a permissive license so any third-party tool, vendor, or community contributor can do the same.

---

## B.1 Design principles

- **JSON Schema described.** A canonical schema lives at `https://goalbindery.app/schemas/blf/v1.json` (reserved). Validators in any language can use it directly.
- **Vendor-neutral.** No GoalBindery-specific fields. A future competitor can implement BLF and import the same packs.
- **Forward-compatible.** Unknown fields are preserved on round-trip; minor versions add fields, never break them.
- **License-aware.** Every pack declares its license (CC-BY, CC-BY-SA, all-rights-reserved, custom). The application surfaces this in the UI and prevents distribution that violates the declared terms.
- **Provenance-preserving.** Every Question and Item Note carries a `source` field documenting where the content came from (user-imported, AI-generated, vendor-published).

---

## B.2 Top-level structure

- **Identity:** `blfVersion`, `kind`, `id`, `name`, `certCode`, `language`, `publisher`, `license`, `createdAt`.
- **Structure:** `subjectGroups` (array, with weights), `tags` (array).
- **Content:** `questions` (array) and `itemNotes` (array).
- **Optional:** `webLinks` (array), `assets` (referenced media files in a sibling directory).

---

## B.3 Example excerpt

A minimal BLF pack for an AWS SAA-C03 Learning Book (excerpt):

```json
{
  "$schema": "https://goalbindery.app/schemas/blf/v1.json",
  "blfVersion": "1.0",
  "kind": "LearningBook",
  "id": "aws-saa-c03-official-sample-2026-q2",
  "name": "AWS SAA-C03 — Official Sample Questions",
  "certCode": "SAA-C03",
  "language": "en",
  "publisher": {
    "id": "pub-aws",
    "name": "Amazon Web Services",
    "url": "https://aws.amazon.com/certification/",
    "verified": true,
    "tier": "partner"
  },
  "analytics": {
    "publisherOptIn": true,
    "minCohortSize": 100,
    "userOptOutHonored": true
  },
  "license": "CC-BY-4.0",
  "createdAt": "2026-04-01T00:00:00Z",
  "subjectGroups": [
    { "id": "sg-1", "name": "Design Secure Architectures",        "weight": 0.30 },
    { "id": "sg-2", "name": "Design Resilient Architectures",      "weight": 0.26 },
    { "id": "sg-3", "name": "Design High-Performing Architectures", "weight": 0.24 },
    { "id": "sg-4", "name": "Design Cost-Optimized Architectures",  "weight": 0.20 }
  ],
  "tags": [
    { "id": "tag-s3", "name": "S3" },
    { "id": "tag-iam", "name": "IAM" }
  ],
  "questions": [
    {
      "id": "q-001",
      "stem": "A company collects telemetry data ...",
      "options": [
        { "key": "A", "text": "Turn on S3 Transfer Acceleration ..." },
        { "key": "B", "text": "Upload to a regional bucket ..." },
        { "key": "C", "text": "Schedule AWS Snowball Edge ..." },
        { "key": "D", "text": "Upload to EC2 with EBS ..." }
      ],
      "correctAnswer": "A",
      "explanation": "S3 Transfer Acceleration uses CloudFront edges ...",
      "subjectGroupId": "sg-3",
      "tagIds": ["tag-s3"],
      "source": "vendor-published",
      "sourceProvenance": {
        "url": "https://aws.amazon.com/.../sample-questions.pdf",
        "page": 4
      }
    }
  ],
  "itemNotes": [
    {
      "id": "n-dynamodb",
      "header": "DynamoDB",
      "details": [
        "Fully managed NoSQL key-value and document database.",
        "Single-digit millisecond latency at any scale.",
        "Provisioned or on-demand capacity modes."
      ],
      "subjectGroupId": "sg-3",
      "tagIds": [],
      "references": [
        "https://docs.aws.amazon.com/dynamodb/"
      ]
    }
  ]
}
```

---

## B.4 Governance and licensing

BLF v1.0 is released under the Creative Commons Attribution 4.0 license, with the schema itself in the public domain. Future versions may move to a neutral foundation if vendor adoption justifies it. The spec lives in a public Git repository (`goalbindery/blf-spec`), accepts community pull requests, and follows semantic versioning.

---

## Open items before promoting to the public repo

- Author the actual JSON Schema file at `schemas/blf/v1.json` (currently reserved, not written).
- Decide whether `analytics` is a top-level object (as in the example) or nested under `publisher` — the example shows it top-level; B.2 doesn't list it. Reconcile.
- Expand the example to show `webLinks` and `assets`, which B.2 mentions but the example omits.
- Confirm CC-BY-4.0 vs CC-BY-SA for the spec text (the proposal mentions both elsewhere).

*Provenance: extracted verbatim from GoalBindery Proposal v0.4 Appendix B, with the schema domain updated from bindery.app to goalbindery.app. Technical content unchanged.*
