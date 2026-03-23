# Problem #1 Dataset: Turning Conversations into Decisions

## Scenario

**Project Meridian** is a platform migration project in which three organizations are collaborating:

- **Acme Health** (client) — migrating from a legacy HL7-MLLP integration layer to FHIR R4 REST APIs
- **DataBridge Systems** (middleware vendor) — designing and building the new integration middleware
- **CloudPeak Infrastructure** (infrastructure partner) — provisioning and configuring the cloud environment

The project has a hard cutover date of **February 14, 2026**, established by Acme's board. Over six weeks, decisions are made, revised, and communicated unevenly across channels — with consequences that accumulate toward the cutover date.

---

## What This Dataset Contains

### `project_manifest.json`
A reference document listing all participants (with org affiliations, titles, and emails) and the three organizations involved. **Start here to understand who is who.**

### `meeting_transcripts/`
Three meeting transcripts in chronological order:

| File | Date | Meeting |
|------|------|---------|
| `transcript_kickoff_jan06.json` | Jan 6 | Project kickoff — scope, approach, and timeline established |
| `transcript_technical_review_jan14.json` | Jan 14 | Technical architecture review |
| `transcript_status_sync_jan28.json` | Jan 28 | Weekly status sync |

Transcripts are raw dialogue only — no pre-extracted action items or decisions.

### `emails/`
Six email threads covering the full project arc:

| File | Dates | Content |
|------|-------|---------|
| `thread_001_kickoff_followup.json` | Jan 6 | Post-kickoff summary from Sarah to all participants |
| `thread_002_mapping_v1_delivery.json` | Jan 15–17 | DataBridge delivers data mapping v1; Acme team review |
| `thread_003_revised_mapping_submission.json` | Jan 20 | Acme sends revised mapping v1.1 to DataBridge |
| `thread_004_vendor_reengagement.json` | Jan 28–29 | Sarah re-engages DataBridge after a period of no contact |
| `thread_005_cloudpeak_scope_conflict.json` | Feb 6 | CloudPeak flags a security configuration issue |
| `thread_006_status_updates_to_diana.json` | Jan 9 – Jan 30 | Four weekly status updates from Sarah to Diana Osei (VP) |

### `slack_threads/`

| File | Channel | Content |
|------|---------|---------|
| `acme_internal_ops_channel.json` | `#meridian-internal` | Acme-only channel. Candid internal discussion not visible to vendors. |
| `acme_databridge_shared_channel.json` | `#meridian-acme-databridge` | Shared channel between Acme and DataBridge teams. |

### `status_updates/status_updates.json`
Five weekly status reports from Sarah Chen to Diana Osei, covering January 9 through February 6.

---

## What Your Solution Should Produce

There is no single required output format. Your solution should demonstrate that it can do one or more of the following, applied to this dataset:

**1. Extract structured project state from the communication stream.**
What decisions have been made — and by whom, with whose input, and whether they were later changed? What action items were committed to, explicitly or implicitly, and what is their current status? What is blocking forward progress?

*Example output form: a structured log of decisions with participants, dates, and current status; an action item register with owners, due dates, and completion state.*

**2. Surface dependencies and the critical path.**
Which tasks depend on other tasks? Which organizations are accountable for which dependencies? Where is the longest sequential chain of dependent work, and which items on that chain are currently at risk?

*Example output form: a dependency graph or table showing what blocks what, with current status per node.*

**3. Detect stalls and missed notifications.**
When did a dependency or action item go silent — and for how long before it was noticed? Were there decisions made in one channel that affected work in another channel, where the affected party was never notified?

*Example output form: a stall log with the dependency, the silence window, when it surfaced, and how it was eventually resolved; a notification gap report.*

The real test: run your solution against a new input. In `test_input/` (see below) you'll find a short additional communication slice — a Slack thread and one email — not from Project Meridian. Your solution should handle it without Project Meridian-specific hardcoding.

---

## Notes on the Data

- **Communication is fragmented across channels.** A decision made in a meeting may be partially referenced in an email and fully absent from the shared Slack channel. Your solution must synthesize across sources.
- **Not all action items are explicit.** Some commitments are implied ("I'll send her a note after this call"), not stated as formal action items.
- **Some decisions were made without full stakeholder attendance.** The attendance lists in each transcript are intentionally uneven.
- **Status reports are written by the person managing the project.** They reflect what that person chose to communicate, which may not fully reflect the state visible in other channels.
- **The data contains no red herrings** — every channel contains real signal — but not every message is equally important.
