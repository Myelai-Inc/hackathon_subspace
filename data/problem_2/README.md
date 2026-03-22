# Problem #2 Dataset: Finding the Work That Matters

## Scenario

**Vertex Solutions Inc.** is a B2B SaaS company providing fleet management software to mid-market logistics companies. The company has 6 Client Success Managers (CSMs), each managing ~23 enterprise accounts.

A time-tracking audit from Q4 2025 found that CSMs spend the majority of their time on assembly work — gathering information, formatting it, logging it, and moving it between systems — rather than on the judgment work that actually retains accounts and drives expansion. CSMs themselves report the split should be inverted.

This dataset gives you the operational role, two core workflows, the system schemas, an integration reference, real-world activity logs, and sample cases to analyze.

---

## What This Dataset Contains

### `role_definition.json`
Defines the CSM role: team size, accounts per CSM, workload metrics, and the list of systems used. **Start here.**

### `workflow_service_escalation.json`
A task-level decomposition of the **Service Escalation Handling** workflow — the highest-frequency, highest-stakes workflow a CSM runs (~18 times/month). Each task includes:
- Description, inputs, and outputs
- Which systems are touched
- Average time spent
- Whether the task requires judgment
- Error rates and error modes where applicable

### `workflow_qbr_preparation.json`
A task-level decomposition of the **Quarterly Business Review (QBR) Preparation and Delivery** workflow (~8 per month per CSM). Same structure as the escalation workflow.

### `systems/`
Schema-level documentation for three of the eight systems in the CSM's stack:

| File | System |
|------|--------|
| `salesforce_crm.json` | Salesforce CRM — account records, activity logs, opportunities |
| `zendesk_ticketing.json` | Zendesk Support — tickets, SLA tracking, organization records |
| `docusign_contracts.json` | DocuSign CLM — contracts, SLA addenda, order forms |

Each file documents the data the system holds, how data is entered, and what each system knows (and doesn't know) about the others.

### `integration_map.json`
A reference document showing what data each system in the CSM stack holds, which integrations currently exist between systems, and what data those integrations do and do not pass.

### `activity_log.csv`
Time-tracking log across 4 CSMs, covering escalations, QBR preparation, onboarding, and renewal management. Each row is a task-level activity with:
- `classification`: `ASSEMBLY`, `ASSEMBLY_JUDGMENT`, or `JUDGMENT`
- `time_spent_minutes`
- Notes (including skipped tasks, workarounds, and deviations from the standard workflow)

### `sample_cases/escalation_cases.json`
Five escalation cases representing the range of complexity CSMs encounter:

| Case | Account | Account Tier | Complexity |
|------|---------|-------------|------------|
| case_001 | Lakeshore Freight Partners | Enterprise | Recurring API failures, churn signal, SLA credit decision |
| case_002 | Meridian Logistics LLC | Professional | Billing dispute near renewal |
| case_003 | Apex Carriers Group | Professional | Pattern detection, unexpected expansion opportunity |
| case_004 | TerraHaul Inc. | Basic | Routine issue, CSM makes triage judgment call |
| case_005 | Northgate Medical Supplies | Enterprise | Executive bypass of ticketing system, compliance implications |

Each case includes: trigger type, complicating factors, SLA risk, resolution outcome, and CSM time breakdown (assembly vs. judgment).

---

## What Your Solution Should Produce

There is no single required output format. Your solution should demonstrate that it can do one or more of the following, applied to this dataset:

**1. Decompose workflows into task units and classify each task.**
For each task in each workflow: is this fully automatable (deterministic, rule-based, no exceptions), partially automatable (structured components that can be templated or AI-assisted, with human review), or human-essential (requires empathy, relationship context, or high-stakes judgment)? Justify your classification — the boundary cases are the interesting ones.

*Example output form: a classification table per workflow with rationale; a rubric that could be applied to a third workflow not in the dataset.*

**2. Detect integration gaps and connect them to downstream assembly work.**
Where should data flow between systems but doesn't? What manual workaround does each gap create, and how much time does that workaround consume? Which gaps are dependencies of other gaps?

*Example output form: a gap map with source system, target system, missing data flow, downstream manual task, and estimated time cost; a dependency ordering of which gaps to fix first.*

**3. Quantify the impact and build a business case.**
Translate your findings into terms an operations leader can act on: staff hours consumed by manual workarounds, error rates from manual data entry, and capacity recovered if the highest-leverage gaps are closed. If you project a throughput change, show the assumptions behind it and where they might break down.

*Example output form: a prioritized gap fix list with estimated labor savings and fix cost per gap; a ROI model with stated assumptions and confidence levels.*

The real test: apply your classification rubric and gap detection logic to `workflow_qbr_preparation.json`. Your solution should generalize — not just describe what's in the escalation workflow.

---

## Notes on the Data

- **The activity log shows real behavior, not ideal behavior.** CSMs skip tasks, pull wrong data, and maintain personal tracking sheets outside the official systems. This is signal, not noise.
- **The sample cases show how judgment is applied under real conditions.** Edge cases (executive escalation bypass, tier-based triage shortcuts) exist for a reason — your classification should account for them.
- **Not everything should be automated.** The goal is to identify where automation creates leverage without degrading the work that actually retains and expands accounts.
- **The system schemas document what each system knows about the others.** Pay attention to what fields exist in one system but are missing or manually maintained in another.
