# Solution Architecture & Build Guide

## Build order

Always build in this sequence — each layer depends on the previous one.

### Stage 1 — Dataverse
Set up the Crisis Assessments table with all columns before building anything else. Every other component reads from and writes to this table.

See: `/dataverse/README.md`

### Stage 2 — Power Apps
Build the Canvas App connected to Dataverse. Test that a submission creates a row in Dataverse before moving on.

See: `/power-apps/README.md`

### Stage 3 — Power Automate
Build the AI analysis flow triggered by Dataverse row creation. Test that the flow fires, AI Builder returns a result, and all four fields are written back to Dataverse.

See: `/power-automate/README.md`

### Stage 4 — Copilot Studio
Build the agent and agent flow. The agent flow writes to Dataverse which triggers the Power Automate flow automatically — no duplicate AI call needed.

See: `/copilot-studio/README.md`

### Stage 5 — Power BI
Connect to Dataverse and build the dashboard once real data exists from stages 1-4.

See: `/power-bi/README.md`

## Key technical decisions

| Decision | Rationale |
|---|---|
| Canvas App not Model-driven | Full layout control for caseworker form |
| Individual controls not Form control | Patch formula gives more control over submission |
| Caseworker as Text not Lookup | Avoids related table, auto-fills from User().FullName |
| AI Builder not Anthropic API | No external API key, stays within M365 tenant |
| Dataverse trigger on Power Automate | Both Power Apps and Copilot Studio submissions trigger the same flow automatically |
| Choice column integers for Risk Level | Dataverse Choice columns require integers not text strings |
| 60 second delay in agent flow | Gives Power Automate time to complete AI analysis before fetching result |
| Vertical Compose steps not parallel | Power Automate can only reference outputs from steps directly above in the same branch |

## Common errors and fixes

| Error | Cause | Fix |
|---|---|---|
| Column does not exist | Column name mismatch between Power Apps and Dataverse | Check exact column names including spaces and capitalisation |
| Risk Level requires Integer | Choice column expects number not text | Map RED=1, AMBER=2, GREEN=3 |
| Split function parameter is Null | AI Builder output path incorrect | Use `body/responsev2/predictionOutput/text` |
| Descending is not valid | Wrong syntax for Power Apps sort | Use `SortOrder.Descending` not `Descending` |
| Output binding not found | Agent flow outputs not synced | Republish agent flow then refresh binding in topic |
| Flow timeout | AI analysis takes longer than expected | Increase delay to 60 seconds, set flow timeout to PT2M |
| LookUp returns blank | Caseworker field value doesn't match | Verify exact value stored in Dataverse matches txtCaseWorker.Text |

## Pipeline stages

| Stage | Status | Component |
|---|---|---|
| 1 — Caseworker submits | Complete | Power Apps + Copilot Studio |
| 2 — AI analysis | Complete | Power Automate + AI Builder |
| 3 — RED alert | Complete | Power Automate + Teams |
| 4 — Manager approval | Complete | Power Automate Approvals |
| 5 — Result to caseworker | Complete | Power Apps Results screen + Copilot Studio |
| 6 — Case closure | Not started | Power Automate + Power Apps |

## Remaining work

- Case closure workflow (Stage 6)
- Power BI dashboard
- Caseworker notification when manager responds
- 2-hour chase flow for unreviewed RED cases
- Correction loop in Copilot Studio agent
