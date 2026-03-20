# Problem #1 Dataset: Turning Conversations into Decisions

## Scenario

**Project Meridian** is a platform migration project in which three organizations are collaborating:

- **Acme Health** (client) — migrating from a legacy HL7-MLLP integration layer to FHIR R4 REST APIs
- **DataBridge Systems** (middleware vendor) — designing and building the new integration middleware
- **CloudPeak Infrastructure** (infrastructure partner) — provisioning and configuring the cloud environment

The project has a hard cutover date of **February 14, 2026**, established by Acme's board. Over six weeks, a series of decisions, miscommunications, untracked action items, and stalls accumulate — ultimately threatening the project timeline.

---

## What This Dataset Contains

### `project_manifest.json`
The master reference document. Contains all participants (with org affiliations and roles), the full dependency graph, and the known decision log — including decisions that were reversed. **Read this first.**

### `meeting_transcripts/`
Three meeting transcripts in chronological order:

| File | Date | Summary |
|------|------|---------|
| `transcript_kickoff_jan06.json` | Jan 6 | Kickoff. Phased migration decided. Action items established. Critical path defined. |
| `transcript_technical_review_jan14.json` | Jan 14 | Technical review. **Phased migration reversed to single-phase.** CloudPeak and Kevin Park not present. |
| `transcript_status_sync_jan28.json` | Jan 28 | Status sync. DataBridge silence surfaces. Tom Reyes departure suspected. Escalation deliberately deferred. |

Each transcript includes structured `action_items_mentioned` and `decisions` arrays alongside the raw dialogue.

### `emails/`
Six email threads covering the full project arc:

| File | Key Content |
|------|-------------|
| `thread_001_kickoff_followup.json` | Post-kickoff action item summary |
| `thread_002_mapping_v1_delivery.json` | DataBridge delivers mapping v1; Linda's review catches significant issues |
| `thread_003_revised_mapping_submission.json` | **Acme sends v1.1 to DataBridge — goes unanswered for 8 days** |
| `thread_004_vendor_reengagement.json` | Tom Reyes departure confirmed; Aisha takes over |
| `thread_005_cloudpeak_scope_conflict.json` | Jordan flags HIPAA security configuration gap for single-phase; Sarah finally escalates to Diana |
| `thread_006_status_updates_to_diana.json` | Five weekly status updates — note how risk is understated across weeks 3 and 4 |

### `slack_threads/`

| File | Channel | Content |
|------|---------|---------|
| `acme_internal_ops_channel.json` | `#meridian-internal` | Acme-only channel. More candid than email — Marcus's LinkedIn discovery, escalation debates, real-time reactions. |
| `acme_databridge_shared_channel.json` | `#meridian-acme-databridge` | Shared channel. Shows the silence from DataBridge, re-engagement, and resolution of the mapping. |

### `status_updates/status_updates.json`
Five weekly status reports from Sarah to Diana. The contrast between the "actual situation" annotations and the reported status is significant signal for stall detection.

---

## Key Analytical Challenges Embedded in This Data

### 1. Decision extraction with reversals
- The phased migration decision (Jan 6) is reversed to single-phase (Jan 14), then partially reversed back to phased (Feb 10, via the CloudPeak security issue)
- The Jan 14 reversal was made without CloudPeak or Kevin Park present
- Diana Osei was not consulted on the Jan 14 reversal — she was informed retroactively in the week 2 status update

### 2. Action items with missing owners or implied deadlines
- Several action items from meeting transcripts were never followed up: the risk comparison one-pager (ai_009), the Priya notification (ai_011), the Diana notification (ai_010)
- The status update cadence (ai_006) was followed, but with understated content

### 3. Stall detection
- DataBridge goes silent Jan 20 → confirmed via Aisha Jan 29 (9 days)
- Kevin Park never responds to two calendar invites (Jan 22, Jan 27) and one Slack message
- CloudPeak's security configuration issue could not surface until they received the architecture doc (Feb 3), but the root cause (single-phase decision) was made Jan 14

### 4. Cross-channel information fragmentation
- Tom's departure is discovered via LinkedIn by Marcus (Slack, Jan 27), confirmed by Aisha (email, Jan 29) — but never formally communicated by DataBridge
- The phasing decision change was made in a meeting (Jan 14), briefly mentioned in an email to CloudPeak (mentioned in status sync Jan 28), and first told to Diana in the week 2 status update
- The real escalation from Marcus ("Diana should know about this") happened in Slack on Jan 27 and Jan 29, not in any official channel

### 5. Incomplete escalation
- Marcus explicitly recommended escalating to Diana on Jan 27 (Slack) and again verbally on Jan 28 (status sync transcript)
- Sarah deferred escalation both times, waiting for more information
- Diana was not escalated to until Feb 6 — after the CloudPeak security issue landed

---

## Suggested Starting Points

- **NLP / Extraction**: Parse the meeting transcripts and emails to extract decisions, action items, and blockers into a structured format. Compare your extraction against the `action_items_mentioned` and `decisions` arrays in each file.
- **Graph modeling**: Build a dependency graph from `project_manifest.json` and populate it with state as you parse communications chronologically. Which nodes are blocked? What is the critical path?
- **Stall detection**: Define a "stall" heuristic (e.g., no response within N days on a critical-path item) and scan the communication stream for matches. What threshold catches the DataBridge silence without generating noise?
- **Escalation analysis**: Map when risks were first visible in the data vs. when they were escalated. What would an automated system have flagged, and when?
