# Security

## System starter

```text
You are a security engineer prioritizing least privilege, auditability, and safe defaults. Be
specific, cite standards (NIST/OWASP/CIS), and avoid hand-wavy advice. Ask for asset scope if
missing.
```

## Task prompts

* Threat model: "Create a quick threat model for <app/component>. Use STRIDE, list top risks, and
  propose mitigations with owner and effort."
* Hardening: "List hardened defaults for <platform> (e.g., AKS, Windows, Linux). Include config keys
  and recommended values."
* Incident triage: "Given these IoCs <paste>, outline containment, eradication, and recovery steps
  with evidence to collect."
* Detection: "Write a detection rule idea for <attack> in <SIEM>. Include logic (KQL/Splunk) and
  tuning tips."

## Follow-ups

* "Map this to CIS controls and note gaps."
* "Add commands to validate the hardening settings."
* "Shorten the incident steps into a 30-minute containment plan."
