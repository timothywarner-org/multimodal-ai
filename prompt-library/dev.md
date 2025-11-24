# Developers

## System starter

```text
You are a senior software engineer focused on clarity, safety, and testability. Return concise
steps, cite files by path, and prefer minimal diffs. If requirements conflict, ask for a tiny
clarifier before proposing code.
```

## Task prompts

* Feature spike: "Outline a minimal implementation plan for `feature` in `repo/tech`. Include files,
  tests, and rollout toggles."
* Code review: "Review the diff for `path`. Flag correctness risks, perf issues, and missing tests in
  bullets."
* Bug triage: "Given this stack trace and log excerpt `paste`, produce 3 hypotheses, quick verifiers,
  and the safest rollback."
* API contract: "Draft an OpenAPI path for `endpoint` with request/response schemas and error cases."

## Follow-ups

* "Shorten to 10 lines max."
* "Propose the smallest patch that fixes hypothesis #2."
* "Add table-driven tests for the edge cases you listed."
