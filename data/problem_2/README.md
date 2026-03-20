# Problem #2 Dataset: Finding the Work That Matters

## Scenario

**Vertex Solutions Inc.** is a B2B SaaS company providing fleet management software to mid-market logistics companies. The company has 6 Client Success Managers (CSMs), each managing ~23 enterprise accounts.

A time-tracking audit from Q4 2025 revealed that CSMs spend **64% of their time on assembly work** — gathering information, formatting it, logging it, and moving it between systems — and only 36% on judgment work (the calls, negotiations, and relationship decisions that actually retain accounts and drive expansion).

CSMs themselves report the split should be inverted.

This dataset gives you the operational role, the workflow, the systems, the integration map, and real-world sample cases to analyze.

---

## What This Dataset Contains

### `role_definition.json`
Defines the CSM role: team size, accounts per CSM, workload metrics, systems used, and the current assembly/judgment time split. **Start here.**

### `workflow_service_escalation.json`
A detailed decomposition of the **Service Escalation Handling** workflow — the highest-frequency, highest-stakes workflow a CSM runs (~18 times/month). Each task includes:
- Inputs and outputs
- Which systems are touched
- Automation classification (`FULLY_AUTOMATABLE`, `PARTIALLY_AUTOMATABLE`, `HUMAN_ESSENTIAL`)
- Average time spent
- Error rates and error modes
- Integration gap references

### `systems/`
Schema-level documentation for each tool in the CSM's stack:

| File | System | Key Content |
|------|--------|-------------|
| `salesforce_crm.json` | Salesforce CRM | Object schemas, custom fields, and known integration gaps |
| `zendesk_ticketing.json` | Zendesk Support | Ticket schema, SLA logic, and sample tickets |
| `docusign_contracts.json` | DocuSign CLM | Contract structure and sample contracts for 4 accounts |

Each system file documents what data flows in and out, and where the seams break.

### `integration_map.json`
The central artifact for integration gap analysis. For each gap:
- What data *should* flow between systems but doesn't
- The current manual workaround
- Time cost per CSM per month
- Error rate of the workaround
- Fix complexity and estimated cost
- Annual labor savings if the gap is closed

**Summary from the integration map:**
- 6 integration gaps identified
- ~16 hours/month per CSM spent on manual workarounds
- ~$82,000/year in labor cost across the 6-CSM team
- Estimated fix cost: ~$25,700 (payback in 3.7 months)
- Throughput model: closing gaps could increase cases handled per CSM from 47 → ~61/month (+30%)

### `activity_log.csv`
Time-tracking log across 4 CSMs, covering escalations, QBR preparation, onboarding, and renewal management. Each row is a task-level activity with:
- `classification`: `ASSEMBLY`, `ASSEMBLY_JUDGMENT`, or `JUDGMENT`
- `time_spent_minutes`
- Notes (including skipped tasks and workarounds)

### `sample_cases/escalation_cases.json`
Five escalation cases representing the range of complexity CSMs encounter:

| Case | Account | Complexity |
|------|---------|------------|
| case_001 | Lakeshore Freight Partners (Enterprise) | Recurring API failures + churn signal + SLA credit decision |
| case_002 | Meridian Logistics LLC (Professional) | Billing dispute near renewal + wrong contract version pulled |
| case_003 | Apex Carriers Group (Professional) | Pattern detection across tickets + unexpected expansion opportunity |
| case_004 | TerraHaul Inc. (Basic) | Routine issue + wrong contract version + CSM skips follow-up |
| case_005 | Northgate Medical Supplies (Enterprise) | Executive bypass of ticketing system + audit compliance implications |

Each case includes: trigger type, complicating factors, SLA risk, resolution outcome, and CSM time breakdown.

---

## Key Analytical Challenges Embedded in This Data

### 1. Workflow decomposition
The `workflow_service_escalation.json` gives you 12 discrete tasks, but the boundaries between them blur in practice. The `activity_log.csv` shows what real task timing looks like, including skips and workarounds. Can you identify which decomposition schema best maps to real behavior?

### 2. Automation classification
Every task has an `automation_classification` with reasoning. But the interesting question is the *boundary* — task t05 (identify specialist) is partially automatable because the routing rule is deterministic, but the specific-person selection involves relationship context. Task t06 (draft internal email) can be templated except for the churn risk assessment. Where do you draw the line, and how do you communicate that distinction to a non-technical audience?

### 3. Integration gap detection and prioritization
The `integration_map.json` gives you 6 gaps with cost data. But they're not independent — gap_001 (Zendesk→Salesforce) and gap_003 (DocuSign→Zendesk via Salesforce) have a logical dependency. Which gaps should be fixed first? Which fix creates the most downstream leverage? How do you quantify the *interaction effects* between gaps?

### 4. Impact quantification
The throughput model in `integration_map.json` projects 47 → 61 cases/month if assembly drops from 64% to 15%. But is that realistic? What assumptions is that model making, and where might it break down? What would make that projection credible to an operations leader?

### 5. Human work that should be protected
Not every assembly task is worth automating. Case_004 shows a CSM skipping follow-up tasks for a Basic tier account — is that good judgment or a failure? The dataset is designed to surface these tradeoffs rather than give you a clean "automate everything" answer.

---

## Suggested Starting Points

- **Workflow decomposition**: Use the `workflow_service_escalation.json` as a baseline and validate it against the `activity_log.csv`. Do the time estimates hold? Where do tasks run together in practice?
- **Automation classification**: Build a classification model or rubric and apply it to the 12 tasks. Then test it against the 5 sample cases — does your classification hold under edge cases like case_005?
- **Integration gap analysis**: Start with `integration_map.json` and build a dependency graph of the 6 gaps. Which fix is the highest-leverage entry point? What's the minimum viable integration set to cut assembly time below 30%?
- **Business case construction**: Take the quantitative data from the integration map and the activity log and build a presentation-ready ROI case. What's the cost to fix, the cost of inaction, and the confidence level of each estimate?
