# Product: SOC Sentinel TH

## One-line pitch
An AI SOC analyst for Thai government agencies: it triages security alerts in seconds,
explains its reasoning, waits for a human to approve any response, and writes the incident
report in Thai.

## Users
- SOC analyst / IT security officer of a government agency (primary)
- IT manager / CISO who reads the Thai incident report (secondary)

## Core flow (keep the scope this small)
1. A finding arrives (from /simulate using seed/findings.json, or POST /findings).
2. The agent triages it: severity (Low/Medium/High/Critical), confidence (0-100),
   MITRE ATT&CK technique, short reasoning in Thai and English.
3. The agent enriches it with tools: IP reputation, related events, MITRE mapping.
4. The agent recommends a playbook (e.g. disable IAM access key, isolate security group,
   block S3 public access) — status becomes PENDING_APPROVAL.
5. A human clicks Approve or Reject on the dashboard. Approve runs containment in DRY_RUN.
6. The agent writes an incident report in Thai (format: summary, timeline, impact,
   actions taken, recommendations, reference to NCSA incident reporting).

## Screens (only these)
- Login (Cognito Hosted UI)
- Dashboard: incident list, severity badges, counters, "Simulate attack" button
- Incident detail: agent timeline (each tool call), recommendation, Approve/Reject,
  Thai report preview with a copy button

## UI language
Thai first, English second. Clean, government-friendly, blue/white.

## Out of scope (do NOT build)
- Real GuardDuty/Security Hub integration (mention as future work)
- Multi-tenant, user management, email/LINE notifications
- Real containment without DRY_RUN
