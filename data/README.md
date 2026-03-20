# Hackathon Dataset — B2B Operations Intelligence

**Myelai Inc. — Space Coast Initiative Hackathon 2026**

This repository contains synthetic datasets for both challenges in the B2B Operations Intelligence problem statement. Each dataset is self-contained and documented.

---

## Problem #1: Turning Conversations into Decisions

**Dataset location:** `problem_1/`

A six-week multi-party project communication stream for a fictional healthcare platform migration ("Project Meridian") involving three organizations. The dataset includes meeting transcripts, email threads, Slack channel logs, and weekly status reports — all containing decisions, action items, blockers, stalls, and a series of escalation failures.

[→ Read the full dataset guide](problem_1/README.md)

**Files:**
```
problem_1/
├── README.md
├── project_manifest.json              # Participants, orgs, dependency graph, decision log
├── meeting_transcripts/
│   ├── transcript_kickoff_jan06.json
│   ├── transcript_technical_review_jan14.json
│   └── transcript_status_sync_jan28.json
├── emails/
│   ├── thread_001_kickoff_followup.json
│   ├── thread_002_mapping_v1_delivery.json
│   ├── thread_003_revised_mapping_submission.json
│   ├── thread_004_vendor_reengagement.json
│   ├── thread_005_cloudpeak_scope_conflict.json
│   └── thread_006_status_updates_to_diana.json
├── slack_threads/
│   ├── acme_internal_ops_channel.json
│   └── acme_databridge_shared_channel.json
└── status_updates/
    └── status_updates.json
```

---

## Problem #2: Finding the Work That Matters

**Dataset location:** `problem_2/`

An operational snapshot of the Client Success Manager role at a fictional B2B SaaS company (Vertex Solutions Inc.). The dataset includes a role definition, a detailed workflow decomposition with automation classifications, system schemas, an integration gap map with cost quantification, a time-tracking activity log, and five sample escalation cases.

[→ Read the full dataset guide](problem_2/README.md)

**Files:**
```
problem_2/
├── README.md
├── role_definition.json               # Role profile, workload metrics, time split
├── workflow_service_escalation.json   # 12-task workflow decomposition with automation classifications
├── integration_map.json               # 6 integration gaps with cost and fix data
├── activity_log.csv                   # Task-level time tracking across 4 CSMs
├── systems/
│   ├── salesforce_crm.json
│   ├── zendesk_ticketing.json
│   └── docusign_contracts.json
└── sample_cases/
    └── escalation_cases.json          # 5 representative escalation cases
```

---

## Design Notes

These datasets are synthetic but modeled on real operational patterns. They are intentionally:

- **Messy**: Information is fragmented across channels. Not all action items are explicitly captured. Some decisions are reversed. Some status updates understate risk.
- **Realistic**: Communication style, timing, and failure modes reflect how real multi-party projects actually go wrong.
- **Rich enough to build on**: Each dataset contains enough structured metadata (action item arrays, automation classifications, integration gap cost data) to validate extractions and cross-check analyses, without being so pre-processed that there's nothing left to solve.

Teams may use these datasets with any stack — NLP pipelines, graph databases, agent frameworks, or full-stack applications.
