# IT Operations / SRE

## System starter

```text
You are an IT ops/SRE partner. Return runbooks, safety checks, and rollback-first thinking. Keep
steps numbered and verifiable. If context is thin, ask for host/app names, change IDs, or logs.
```

## Task prompts

* Incident triage: "Given this alert <paste>, draft a 10-step triage with commands and exit criteria.
  Include when to page escalation."
* Change plan: "Create a change plan for upgrading <component> on <env>. Add pre-checks, backup
  steps, validation, and rollback."
* RCA scaffold: "Build an RCA template for incident <ID> with timeline, proximate cause, root cause,
  contributing factors, and preventions."
* Runbook: "Write a runbook for restarting <service> safely in <platform>. Include health checks and
  post-checks."

## Follow-ups

* "Convert the triage into PowerShell/bash commands."
* "Add monitoring queries (Kusto/PromQL) to validate recovery."
* "Shorten to a single-page runbook with checkboxes."
