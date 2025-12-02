# Basic Foundry Agent Tutorial

Get a working conversational agent into users' hands with Microsoft Foundry. You will stand up
a project, add grounded knowledge, connect a simple tool, and publish an endpoint you can call
from Teams, Power Platform, or any HTTP client.

## Prerequisites

* Azure subscription with Microsoft Foundry access and billing configured
* Role: **Contributor** on the target project
* Model access approved (GPT-4o, GPT-4o mini) with deployment quota
* One knowledge source to ground on (SharePoint library, website, or PDF set)
* Optional: REST API or Azure Function to expose as a tool

## Architecture at a glance

1. **Project**: Workspace that holds model deployments, knowledge bases, and agents
1. **Connections**: Links to Azure AI Search, storage, and data sources
1. **Knowledge Base (Foundry IQ)**: Searchable store powered by Azure AI Search for grounding
1. **Agent**: System prompt + tools + grounding rules exposed via chat endpoint
1. **Channels**: Callable from Teams, Power Apps, or custom apps via HTTPS

## Step 1: Create a project

1. Sign in to [Microsoft Foundry](https://ai.azure.com) and select **Create an agent**.
1. Enter a project name; an account and project are created automatically.
1. Wait for provisioning to complete; you'll land in the agent playground.

> **Note**: This fast path creates a Foundry project. For hub-based projects with advanced
> features like prompt flow or managed compute, use **Create hub** instead.

## Step 2: Deploy a model

1. Open **Models + endpoints** > **Deploy model** (or use the model catalog).
1. Select **gpt-4o-mini** for fast iteration (upgrade to **gpt-4o** later for quality).
1. Accept the default deployment name (e.g., `gpt-4o-mini`) and complete deployment.

## Step 3: Add grounded knowledge

1. Navigate to **Data** > **Create knowledge source**.
1. Choose **Upload files** and add a starter set (5-20 documents) for your domain.
1. Configure **Chunk + embed** settings; keep defaults for chunk size (512) and overlap (50).
1. When indexing completes, note the **Knowledge Base ID** (you'll link it to the agent).

> **Tip**: Knowledge sources use Azure AI Search indexes. You can also connect existing
> search indexes or web sources via Foundry IQ.

## Step 4: Create the agent

1. In **Agents** > **Create agent**, name it after a task (e.g., **Project Q&A Copilot**).
1. In **System prompt**, define tone and boundaries:

   ```text
   You are a concise assistant for project stakeholders. Use indexed content first.
   If unsure, say so and ask a clarifying question. Cite document titles when possible.
   ```

1. Under **Grounding**, select your knowledge base and enable **Citations**.
1. Choose your deployed model (e.g., `gpt-4o-mini`).

## Step 5: Add a tool (optional but recommended)

Expose a REST API or Azure Function as a tool so the agent can take action. Example OpenAPI
schema for a support ticket function:

```json
{
  "name": "createSupportTicket",
  "description": "Create a support ticket with a subject and priority.",
  "parameters": {
    "type": "object",
    "properties": {
      "subject": { "type": "string" },
      "priority": { "type": "string", "enum": ["low", "medium", "high"] }
    },
    "required": ["subject", "priority"]
  }
}
```

1. In **Tools**, add **OpenAPI Spec** or **Azure Functions** and paste your API URL and schema.
1. Provide API key header if needed and test the connection.

> **Important**: Function calling is NOT supported in the portal playground. Test tools via
> SDK or API calls to the deployed agent endpoint.

## Step 6: Test the agent

1. Open **Agent Playground** and try a starter prompt (e.g., "Summarize the Release A timeline").
1. Verify citations appear and link to correct sources.
1. Adjust the system prompt if responses are too verbose or off-topic.

> **Note**: To test function calling, deploy the agent and call it via SDK or REST API.

## Step 7: Publish and consume

1. Select **Deploy** to create a managed endpoint for your Foundry project.
1. Copy the **Endpoint URL** and **API Key** to use from your application.
1. Quick test via `curl`:

   ```bash
   curl -X POST "$ENDPOINT" \
     -H "Content-Type: application/json" \
     -H "api-key: $KEY" \
     -d '{"messages":[{"role":"user","content":"Give me the release summary"}]}'
   ```

1. Integrate the endpoint into Power Apps, Power Automate, or Teams message extensions.

> **API Version**: Use `2025-05-01` (GA) or `2025-05-15-preview` for the latest features.

## Success criteria checklist

* Agent answers from indexed content with accurate citations
* Tool calls succeed and return structured results (test via API, not portal playground)
* Latency for common queries < 5 seconds on `gpt-4o-mini`
* Agent asks clarifying questions when content is missing (no hallucinations)

## Next steps

* Add more knowledge sources (OneLake, SharePoint, web) and refresh indexes on a schedule
* Upgrade to **GPT-4o** for higher quality once prompts and data are validated
* Add evaluations in **Tracing + evals** to catch regressions before production
* Explore advanced tools: Browser Automation, Azure Logic Apps, MCP endpoints
