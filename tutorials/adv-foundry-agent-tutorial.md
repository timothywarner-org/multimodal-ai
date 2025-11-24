# Advanced Foundry Agent Tutorial

Ship a production-grade Azure AI Foundry agent with strong grounding, tool orchestration,
enterprise guardrails, and monitoring. This builds on the basics to add vector search patterns,
function calling, evaluations, and rollout discipline.

## Prerequisites

* Same setup as the basic tutorial plus:
  * Access to Azure AI Search or built-in vector indexes for RAG
  * Ability to publish HTTPS APIs for tool calls (managed identity or API key)
  * Azure Application Insights (or Log Analytics) for telemetry
  * Approval to run evaluations against sample transcripts

## High-level architecture

* **Model**: GPT-4o for quality; GPT-4o mini for latency-sensitive paths
* **Grounding**: Vector index + optional keyword search fallback
* **Tools**: HTTP endpoints (REST or OpenAPI) with schema-defined parameters
* **Policies**: Safety, PII detection, topic filters, and citation requirements
* **Observability**: Built-in traces plus App Insights for alerts and dashboards

## Step 1: Harden the system prompt

1. Capture purpose, tone, and refusal rules explicitly. Example:

   ```text
   You are an internal operations copilot. Always ground answers in indexed content and cite titles.
   Refuse finance, HR, or legal questions outside the provided corpus. Ask one clarifying question
   when intent is ambiguous. Keep answers under 8 sentences.
   ```

1. Add 3-5 **behavior examples** (good and bad) so the model learns the pattern.

## Step 2: Upgrade grounding with hybrid search

1. Create a **Vector + keyword** index to handle both semantic and exact matches.
1. Tune chunk size (500-800 tokens) and overlap (10-15%) for richer context.
1. Enable **filters** (metadata like department, region) to keep answers scoped.
1. Turn on **Citations** and require at least one source per fact.

## Step 3: Add multi-tool orchestration

1. Define tools for CRUD operations and system lookups with explicit JSON schemas.
1. Use descriptive names and short descriptions so the model chooses correctly.
1. Provide **tool examples** in the agent definition (when to call, sample inputs/outputs).
1. Prefer managed identity for auth; fall back to API keys only when unavoidable.

## Step 4: Build a RAG + tool pattern

1. In **Grounding**, enable your hybrid index.
1. In **Tools**, register an action like `createChangeRequest` that writes to your ITSM API.
1. Add a **policy** that forces citations before tool execution for high-risk topics.
1. Test prompts where the agent first cites context, then offers to execute the tool.

## Step 5: Evaluate quality and safety

1. Create a **Dataset** of 20-30 transcripts covering happy paths and edge cases.
1. Run **Evaluation jobs** for relevance, citation accuracy, toxicity, and refusal quality.
1. Set pass thresholds (e.g., relevance > 0.7, citations present in 95% of answers).
1. Repeat after each prompt or tool change; treat failures as blockers to deploy.

## Step 6: Instrument telemetry

1. Enable **Logging** to Application Insights from the agent settings.
1. Capture latency, tool-call failures, refusal counts, and grounding hit rate.
1. Build a workbook with:
   * Latency by prompt type
   * Tool failure rates by endpoint
   * Percentage of answers with citations
   * Top refusal reasons
1. Configure alerts for failure spikes and missing-citation anomalies.

## Step 7: Secure and govern

1. Apply **Network restrictions** so only allowed callers hit the endpoint.
1. Use **Role assignments** for least-privilege access to the project and data stores.
1. Turn on **Content filters** (violence, self-harm, sexual, hate) with strict settings.
1. Document data flows and retention for compliance (who can see prompts/responses).

## Step 8: Ship with confidence

1. Promote changes through **environments** (dev > test > prod) using IaC where possible.
1. Smoke-test with `curl` or Postman using recorded golden prompts.
1. Create a **rollback plan** (previous prompt version + prior tool schema) before go-live.
1. Share a user-facing quick start that includes privacy notes and best prompts.

## Production readiness checklist

* Prompt defines scope, tone, citations, and refusal behavior
* Grounding uses hybrid search with filters; citations required
* Tools use secure auth, clear schemas, and error handling is tested
* Evaluations pass for quality and safety before deployment
* Telemetry and alerts active for latency, tool errors, and missing citations
* Rollback plan documented and validated

## Next steps

* Add **A/B prompt testing** to compare instruction variants
* Introduce **cost controls** by routing short queries to GPT-4o mini
* Layer **retrieval caches** to reduce latency for repeated asks
