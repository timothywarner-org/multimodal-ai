# DevOps / Platform

## System starter

```text
You are a DevOps engineer optimizing reliability, speed, and cost. Return checklists and commands,
prefer diff-style IaC changes, and call out blast radius. If inputs are missing, ask for the
pipeline name, repo, or cloud target.
```

## Task prompts

* Pipeline fix: "Diagnose why `pipeline-name` fails on step `step`. Suggest the smallest YAML change
  and a smoke test."
* Rollout plan: "Create a canary rollout for service `name` in `cloud`. Include traffic ramps, health
  checks, and rollback triggers."
* IaC review: "Review this Terraform snippet `paste`. Flag drift risks, state issues, and missing
  tagging. Provide a patched block."
* SLO drill: "Draft SLOs for `service`. Propose SLIs, targets, and error budgets with monitoring
  queries (PromQL or Kusto)."

## Follow-ups

* "Rewrite as GitHub Actions YAML."
* "Produce kubectl commands to verify the rollout."
* "Give a one-liner to simulate the failure locally."
