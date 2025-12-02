# Advanced Foundry Agent Tutorial

Ship a production-grade Azure AI Foundry agent with strong grounding, function calling,
enterprise guardrails, and observability. This builds on the basics to add hybrid search
patterns, multi-tool orchestration, evaluations, and rollout discipline.

## Prerequisites

* Same setup as the basic tutorial plus:
  * Azure AI Search or Foundry IQ vector indexes for RAG
  * HTTPS APIs for function calling (managed identity or API key auth)
  * Azure Application Insights for OpenTelemetry traces
  * Approval to run safety evaluations against sample transcripts

## High-level architecture

* **Model**: GPT-4o for quality; GPT-4o mini for latency-sensitive paths
* **Grounding**: Hybrid search (vector + keyword) with Reciprocal Rank Fusion (RRF)
* **Tools**: Function calling with Azure Functions or OpenAPI endpoints
* **Safety**: Azure AI Content Safety filters + groundedness detection
* **Observability**: OpenTelemetry traces with Application Insights agent view

## Step 1: Harden the system prompt

1. Capture purpose, tone, and refusal rules explicitly. Example:

   ```text
   You are an internal operations copilot. Always ground answers in indexed content and cite
   titles. Refuse finance, HR, or legal questions outside the provided corpus. Ask one
   clarifying question when intent is ambiguous. Keep answers under 8 sentences.
   ```

1. Add 3-5 **behavior examples** (good and bad) so the model learns the pattern.

## Step 2: Configure hybrid search with Azure AI Search

1. Create a **hybrid index** (vector + keyword) that runs by default in Azure AI Search tool.
1. Tune chunk size (500-800 tokens) and overlap (10-15%) for richer context.
1. Enable **metadata filters** (department, region) to keep answers scoped.
1. Turn on **Citations** and require at least one source per fact.
1. Optional: Enable **semantic ranking** for highest relevance (vector + keyword + semantic).

## Step 3: Add function calling for multi-tool orchestration

1. Define functions with explicit JSON schemas (descriptive names and short descriptions).
1. Use models released after Nov 6, 2023 for parallel function calling support.
1. Register tools like `createChangeRequest` that call your ITSM or Azure Functions APIs.
1. Prefer **managed identity** for auth; fall back to API keys only when unavoidable.
1. Note: Function runs expire after 10 minutes; submit tool outputs promptly.

## Step 4: Build an agentic RAG + tool pattern

1. In **Grounding**, enable your Foundry IQ or Azure AI Search hybrid index.
1. In **Function calling**, register actions that write to external systems.
1. Add a **policy** that forces citations before tool execution for high-risk topics.
1. Test prompts where the agent first cites context, then offers to execute the tool.
1. Leverage **agentic retrieval** for complex queries with parallel subquery execution.

## Step 5: Evaluate quality and safety

1. Create a **Dataset** of 20-30 transcripts covering happy paths and edge cases.
1. Run **Evaluation jobs** using Azure AI Foundry's GPT-4o-based evaluators:
   * **AI quality (AI-assisted)**: relevance, coherence, fluency
   * **AI quality (NLP)**: groundedness, citation accuracy (requires ground truth)
   * **Risk & safety**: hate, violence, sexual, self-harm, protected material, jailbreak
1. Use **PyRIT framework** for adversarial AI red teaming simulations.
1. Set pass thresholds (relevance > 0.7, citations present in 95% of answers).
1. Repeat after each prompt or tool change; treat failures as blockers to deploy.

## Step 6: Instrument OpenTelemetry traces

1. Associate an **Application Insights** resource in Foundry portal's Tracing section.
1. Enable **OpenTelemetry** instrumentation using Azure Monitor OpenTelemetry Distro.
1. Capture latency, function call failures, refusal counts, and grounding hit rate.
1. Use the **Agents details** unified view in Application Insights to monitor:
   * Latency by prompt type
   * Function call failure rates by endpoint
   * Percentage of answers with citations
   * Token usage and costs
1. Configure alerts for failure spikes and missing-citation anomalies.

## Step 7: Apply content safety and governance

1. Apply **Azure AI Content Safety** filters (violence, hate, sexual, self-harm):
   * Default filters at **medium severity** threshold for prompts and completions
   * Four severity levels: safe, low, medium, high
1. Enable **groundedness detection** to ensure responses based on trusted sources.
1. Use **ProtectedMaterialEvaluator** to check for copyrighted content (lyrics, articles).
1. Apply **Network restrictions** so only allowed callers hit the endpoint.
1. Use **Role assignments** (RBAC) for least-privilege access to projects and data stores.
1. Document data flows and retention for compliance (prompt/response visibility).

## Step 8: Deploy with enterprise discipline

1. Promote through **environments** (dev > test > prod) using separate resource groups.
1. Choose deployment type:
   * **Standard**: ideal for dev/test, flexible setup
   * **Provisioned**: reserved throughput for production
   * **Global Provisioned**: dynamic routing to best-availability datacenter
   * **Data Zone Provisioned**: explicit data residency requirements
1. Smoke-test with `curl` or Postman using recorded golden prompts.
1. Create a **rollback plan** (previous prompt version + prior function schema).
1. Share a user-facing quick start that includes privacy notes and best prompts.

## Production readiness checklist

* Prompt defines scope, tone, citations, and refusal behavior
* Grounding uses hybrid search (vector + keyword) with RRF; citations required
* Function calling uses secure auth, clear schemas, and error handling is tested
* Evaluations pass for quality (relevance, coherence) and safety (content risks)
* OpenTelemetry traces active in Application Insights with alerts configured
* Content safety filters enabled at medium severity with groundedness detection
* Deployment type matches workload (standard vs provisioned vs global)
* Rollback plan documented and validated

## Next steps

* Add **A/B prompt testing** to compare instruction variants
* Introduce **cost controls** by routing short queries to GPT-4o mini
* Layer **retrieval caches** to reduce latency for repeated asks
* Explore **connected agents** (GA) for multi-agent systems without external orchestrators
