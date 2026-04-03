# Adult Social Care — Crisis Prevention Solution

An AI-powered crisis prevention tool for adult social care caseworkers, built entirely on Microsoft Power Platform with no code.

## What it does

Caseworkers submit case assessments either through a Power Apps form or by chatting with an AI agent in Microsoft Teams. The system automatically analyses the case, assigns a RED / AMBER / GREEN risk rating, notifies the duty manager, and guides the caseworker through next steps — all within seconds of submission.

## Architecture

```
Caseworker (Power Apps or Copilot Studio)
        ↓
Dataverse (Crisis Assessments table)
        ↓
Power Automate (AI analysis + alerts)
        ↓
AI Builder (risk assessment)
        ↓
Duty Manager (Teams approval)
        ↓
Power BI (team dashboard)
```

## Components

| Component | Purpose |
|---|---|
| Power Apps Canvas App | 4-screen form for caseworkers to submit assessments |
| Dataverse | Central data store for all assessments and AI outputs |
| Power Automate | Triggers AI analysis, parses results, sends alerts |
| AI Builder | Generates risk rating, summary, actions and escalation draft |
| Copilot Studio | Conversational AI agent for Teams |
| Power BI | Live risk dashboard for managers and the team |

## Risk levels

| Level | Meaning | Response |
|---|---|---|
| RED | Immediate risk to life or serious harm | Same-day response, duty manager approval required |
| AMBER | Elevated risk, urgent review needed | Review within 72 hours |
| GREEN | Monitored risk, no immediate danger | Routine follow-up |

## Safeguarding

This tool is designed to **support** professional decision-making, not replace it. Every assessment includes a mandatory disclaimer. The caseworker and their manager retain full legal responsibility for all decisions under the Care Act 2014.

## Build order

1. Dataverse table
2. Power Apps form
3. Power Automate flow
4. Copilot Studio agent
5. Power BI dashboard

## Licence

Private — for internal use only.
