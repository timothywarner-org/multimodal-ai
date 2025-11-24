# Advanced Copilot Studio Agent Tutorial

Go beyond basics with Copilot Studio. Build a production-ready agent that mixes grounding,
Power Platform actions, ALM, and observability, then ship it to Teams and the web.

## Prerequisites

* Environment with Copilot Studio + Power Platform (prod and a sandbox)
* Maker permissions in sandbox; deployment pipeline access for prod
* Access to the data sources you will ground on (SharePoint, Dataverse, Azure SQL)
* An Application Insights resource for telemetry (optional but recommended)

## Solution design blueprint

1. **Persona + guardrails**: Tone, scope, what to avoid
1. **Knowledge**: SharePoint libraries, Dataverse tables, or websites
1. **Actions**: Power Automate flows, Dataverse actions, and custom connectors
1. **Channels**: Teams app, web experience, or custom canvas app
1. **Observability**: Turn on trace logging to Application Insights

## Step 1: Author robust system instructions

1. In the unified canvas, open **Instructions** and define:
   * Role and audience ("You are a benefits concierge for HR")
   * Boundaries (no legal advice, no payroll updates)
   * Style (brief, cite sources, ask 1 clarifying question if unsure)
1. Add **Conversation starters** that mirror top user intents (4-6 entries).

## Step 2: Ground with trusted data

1. Add **Data sources** starting with the most authoritative content.
1. Prioritize SharePoint libraries or Dataverse tables over broad websites.
1. Turn on **Citations** and test snippets to ensure grounding works.
1. For sensitive data, apply **Data loss prevention** policies that restrict connectors.

## Step 3: Wire actions with Power Automate

1. Create a flow using the **Copilot action** trigger.
1. Define clear inputs (subject, urgency, description) and outputs (ticketId, status, url).
1. Return a compact JSON payload so the agent can summarize results.
1. In Copilot Studio **Actions**, import the flow, map parameters, and describe when to use it.
1. Test in the canvas to verify tool calls and error handling paths.

## Step 4: Add a custom connector (optional)

1. Build a connector for your REST API; enforce OAuth or API key auth.
1. Mark actions as **Connector actions** in the agent so the model knows how to call them.
1. Provide example prompts inside the connector definition for better grounding.

## Step 5: Use variables and memory intentionally

* Use **Conversation memory** for short-term context; reset on sensitive flows.
* Store durable state in Dataverse tables rather than long-term memory.
* Pass outputs from one action into another via variables instead of re-asking the user.

## Step 6: Apply evaluation and safety

1. Create **Test cases** for the top 10 user tasks (happy path + boundary cases).
1. Use **Prompt testing** to check citation quality and refusal behavior.
1. Enable **Content moderation** and **PII detection**; add disclaimers when PII is present.

## Step 7: Package and move with ALM

1. Add the agent and flows to a **Solution** in the sandbox environment.
1. Use **Deployment pipelines** to promote to test and prod with approvals.
1. Version your system instructions and keep change logs in the solution notes.

## Step 8: Instrument and observe

1. Turn on **Monitor** to capture transcript-level diagnostics in the maker portal.
1. Connect to **Application Insights** for queryable telemetry (latency, tool errors, refusals).
1. Define alerts for failure spikes and high refusal rates.

## Step 9: Publish to channels

1. Ship to **Teams** with a clear app name, icon, and short description.
1. Add **Web experience** if you need anonymous or external user access.
1. Provide a one-page quick start with best prompts and privacy notice.

## Operational checklist

* Instructions state scope, tone, and refusal rules
* Grounding uses authoritative sources with citations on
* Actions return structured outputs and handle errors gracefully
* DLP policies applied; no risky connectors in production
* Telemetry enabled with alerts for failures and refusals
* Deployed via solutions/pipelines, not ad-hoc

## Next steps

* Add regression suites that run before each publish
* Introduce A/B tests for alternative prompts or action ordering
* Pair with **Microsoft Purview** for richer data governance across sources
