# Advanced Copilot Studio Agent Tutorial

Go beyond basics with Copilot Studio. Build a production-ready agent that uses generative
orchestration, knowledge sources, Power Platform tools, ALM, and observability, then publish it
to Teams and the web.

## Prerequisites

* Environment with Copilot Studio and Power Platform (production and a sandbox)
* Maker permissions in sandbox; deployment pipeline access for production
* Access to data sources for grounding (SharePoint, Dataverse, Azure SQL)
* An Application Insights resource for telemetry (optional but recommended)

## Solution design blueprint

1. **Instructions and guardrails**: Tone, scope, boundaries, and refusal rules
1. **Knowledge sources**: SharePoint libraries, Dataverse tables, or websites with descriptions
1. **Tools**: Agent flows (Power Automate), connectors, and other agents
1. **Channels**: Teams app, web experience, or custom canvas app
1. **Observability**: Monitor dashboard and Application Insights telemetry

## Step 1: Author robust instructions

1. In the agent authoring canvas, open **Instructions** and define:
   * Role and audience ("You are a benefits assistant for HR employees")
   * Boundaries (no legal advice, no payroll modifications)
   * Style (brief, cite sources, ask clarifying questions when uncertain)
1. Add **Conversation starters** that mirror top user intents (4-6 examples).
1. Generative orchestration is on by default, allowing the agent to select the best topics,
   tools, and knowledge sources automatically.

## Step 2: Add knowledge sources with descriptions

1. Navigate to **Knowledge** and add data sources starting with the most authoritative.
1. Prioritize SharePoint libraries or Dataverse tables over broad website searches.
1. **Critical**: Provide clear descriptions for each knowledge source. With generative
   orchestration, the AI uses descriptions to select the top 4 knowledge sources per query.
1. Enable **Citations** and test to ensure grounding works correctly.
1. Note: Generative orchestration doesn't support Bing Custom Search at the agent level; use
   those in a Generative answers node within a topic if needed.

## Step 3: Wire tools with agent flows

1. In Power Automate, create a flow using the **Run a flow from Copilot** trigger (previously
   "Copilot action").
1. Define clear inputs (subject, urgency, description) and outputs (ticketId, status, url).
1. Return a compact JSON payload so the agent can summarize results naturally.
1. In Copilot Studio, go to **Tools** > **Add a tool** > **Agent flow**.
1. Select your flow, provide a clear description of when to use it, and map parameters.
1. Test in the canvas to verify the agent calls the tool correctly and handles errors.

## Step 4: Add custom connectors as tools

1. Go to **Tools** > **Add a tool** > **New tool** > **Custom connector**.
1. Build a connector for your REST API; enforce OAuth or API key authentication.
1. Provide example prompts and clear descriptions in the connector definition.
1. The agent's generative orchestration will automatically invoke the connector when relevant.

## Step 5: Use variables and conversation memory

* Use **Conversation memory** for short-term context; reset on sensitive flows.
* Store durable state in Dataverse tables rather than relying on long-term memory.
* Pass outputs from one tool into another via variables instead of re-prompting the user.
* With generative orchestration, the agent automatically fills in inputs using conversation
  history.

## Step 6: Test and apply content safety

1. Create **Test cases** for the top 10 user tasks (happy path and boundary cases).
1. Use the **Test agent** pane to validate citation quality and refusal behavior.
1. Enable **Content moderation** in Settings to filter harmful content.
1. For sensitive information, test PII detection and add disclaimers when PII is present.

## Step 7: Package and deploy with ALM

1. In Copilot Studio, go to **Settings** > **Solutions** to add the agent and flows to a
   solution in your sandbox environment.
1. Use in-product **Deployment pipelines** to promote to test and production environments with
   approvals. AI-generated deployment notes are available to document changes.
1. Alternatively, export solutions manually or use Azure DevOps/GitHub for source-controlled
   deployments.
1. Version your instructions and keep change logs in solution metadata.

## Step 8: Instrument with observability

1. In Copilot Studio, navigate to **Monitor** to view transcript-level diagnostics and
   conversation analytics.
1. Connect to **Application Insights** for queryable telemetry using Kusto queries.
   * The built-in dashboard view shows key metrics: total conversations, latency, exceptions,
     tool usage, and topic analytics.
   * The unified Agents view in Application Insights monitors agents across Azure AI Foundry,
     Copilot Studio, and third-party sources.
1. Query logs to analyze distinct users, tool invocations, and errors.
1. Define alerts for failure spikes, high refusal rates, and latency thresholds.

## Step 9: Apply security and governance

1. Configure **Data loss prevention (DLP) policies** in Power Platform admin center to:
   * Restrict which knowledge sources agents can access
   * Block specific connectors or tools in production environments
   * Require user authentication before publishing
   * Control publishing channels and event trigger usage
1. Integrate **Microsoft Purview** to scan knowledge sources (like SharePoint) and restrict
   agents from processing sensitive content based on sensitivity labels.
1. Disable agent publishing with generative AI features for specific tenants if needed.

## Step 10: Publish to channels

1. Publish to **Teams** with a clear app name, icon, and short description.
1. Enable **Web** channel if you need anonymous or external user access.
1. Provide a quick start guide with sample prompts and a privacy notice.
1. Consider mobile and custom canvas app integration for specialized scenarios.

## Operational checklist

* Instructions state scope, tone, and refusal rules
* Knowledge sources have clear descriptions for AI selection
* Tools (flows and connectors) return structured outputs and handle errors
* DLP policies applied; no risky connectors in production
* Telemetry enabled with Application Insights integration
* Deployed via solutions and pipelines, not ad-hoc exports
* Content moderation and authentication configured

## Next steps

* Build regression test suites that run before each deployment
* Use A/B testing for alternative instructions or tool orchestration strategies
* Integrate **Microsoft Purview** for advanced data governance across all sources
* Monitor the unified Agents view in Application Insights for cross-platform insights
* Explore tenant graph grounding with semantic search for Microsoft 365 Copilot integration
