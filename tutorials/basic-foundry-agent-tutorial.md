# Basic Foundry Agent Tutorial

Get a working conversational agent into users' hands with Azure AI Foundry. You will stand up a
project, add grounded knowledge, connect a simple tool, and publish an endpoint you can call from
Teams, Power Platform, or any HTTP client.

## Prerequisites

* Azure subscription with Azure AI Foundry enabled and billing set
* Role: **Contributor** on the target hub and project
* Model access approved (e.g., GPT-4o, GPT-4o mini) and a deployment quota available
* One knowledge source to ground on (SharePoint library, website, or set of PDFs)
* Optional: a REST API you can expose as a tool (sample included below)

## Architecture at a glance

1. **Hub + Project**: Workspace that holds your model deployments, indexes, and evaluations
1. **Connections**: Links to storage and data sources
1. **Index**: Searchable store for your documents to ground the model
1. **Agent**: System prompt + tools + grounding rules exposed via chat endpoint
1. **Channels**: Call from Teams, Power Apps, or a custom app via HTTPS

## Step 1: Create a hub and project

1. Sign in to Azure AI Foundry and select **Create hub**.
1. Choose region close to your data; enable managed identity.
1. After the hub is ready, create a **Project** inside it (this scopes data and deployments).

## Step 2: Deploy a model

1. Open **Models + endpoints** > **Deploy model**.
1. Pick **GPT-4o mini** for fast iteration (switch to **GPT-4o** later for higher quality).
1. Accept default deployment name (for example, `gpt-4o-mini`) and finish.

## Step 3: Add grounded knowledge

1. Go to **Data** > **Add index**.
1. Select **Files** and upload a small starter set (5-20 docs) that represent your domain.
1. Choose **Chunk + embed** to vectorize content; keep defaults for chunk size and overlap.
1. When indexing completes, note the **Index ID** (you will bind it to the agent).

## Step 4: Create the agent

1. Navigate to **Agents** > **Create agent**.
1. Name it after a task, e.g., **Project Q&A Copilot**.
1. In **System prompt**, set tone and boundaries:

   ```text
   You are a concise assistant for project stakeholders. Use the indexed content first.
   If unsure, say so and ask a clarifying question. Cite document titles when possible.
   ```

1. Under **Grounding**, select your index and enable **Citations**.
1. Choose your deployed model (e.g., `gpt-4o-mini`).

## Step 5: Add a simple tool (optional but recommended)

Expose a small REST API as a tool so the agent can take action. Example schema:

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

1. In **Tools**, choose **HTTPS endpoint** and paste your API URL and JSON schema.
1. Provide an API key header if needed and test the connection.

## Step 6: Test the agent

1. Open **Playground** and start with a starter prompt (for example, "Summarize the Release A
   timeline").
1. Verify citations appear; click them to confirm the source.
1. Ask the agent to call your tool ("Log a ticket to track printer outage, priority high").
1. Adjust the system prompt if responses are too long or off-topic.

## Step 7: Publish and consume

1. Select **Deploy** to create a managed endpoint.
1. Copy the **Endpoint URL** and **Key** to use from your app.
1. Quick test via `curl`:

   ```bash
   curl -X POST "$ENDPOINT" \
     -H "Content-Type: application/json" \
     -H "api-key: $KEY" \
     -d '{"messages":[{"role":"user","content":"Give me the release summary"}]}'
   ```

1. Wire the endpoint into Power Apps, Power Automate, or a Teams message extension.

## Success criteria checklist

* Agent answers from your indexed content with citations
* Tool calls succeed and return structured results
* Latency for common questions < 5 seconds on `gpt-4o-mini`
* No hallucinated answers when content is missing; agent asks clarifying questions

## Next steps

* Add more sources (OneLake, SharePoint, websites) and re-index on a schedule
* Switch to **GPT-4o** for higher quality once prompts and data are stable
* Add evaluations in **Safety + Quality** to catch regressions before shipping
