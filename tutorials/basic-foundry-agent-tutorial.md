# Build Your First AI Agent with Azure AI Foundry

## Overview

Azure AI Foundry (formerly Azure AI Studio) is an enterprise platform for building, evaluating, and
deploying generative AI applications and agents. This tutorial walks you through creating your first
intelligent agent using Azure's tools and models, from initial setup through deployment and testing.

**Estimated time to complete**: 60-75 minutes

## Prerequisites

Before you begin, ensure you have:

* Active Azure subscription with contributor or owner access
* Azure AI Foundry hub and project created (or permissions to create them)
* Basic understanding of APIs and JSON
* Familiarity with Azure portal navigation
* Optional: Visual Studio Code with Azure extensions

## Learning Objectives

By the end of this tutorial, you'll be able to:

* Set up an Azure AI Foundry project and configure resources
* Understand agent architecture and components in Azure AI Foundry
* Build a basic agent using pre-built connectors and tools
* Test and refine agent responses through the playground
* Deploy your agent and integrate it with applications
* Monitor agent performance and usage metrics

## What is Azure AI Foundry

Azure AI Foundry is an integrated development environment for AI applications that provides:

* **Model catalog**: Access to Azure OpenAI, open-source models, and custom models
* **Agent builder**: Low-code interface for creating intelligent agents
* **Prompt flow**: Visual designer for orchestrating AI workflows
* **Evaluation tools**: Built-in metrics for testing agent quality
* **MLOps integration**: Enterprise-grade deployment and monitoring
* **Safety guardrails**: Content filtering and responsible AI controls

### Agent Capabilities

Azure AI Foundry agents can:

* **Understand intent**: Parse user queries and extract meaning
* **Retrieve knowledge**: Search documents, databases, and web sources
* **Execute actions**: Call APIs, run code, and trigger workflows
* **Maintain context**: Track conversation history across turns
* **Generate responses**: Synthesize information into natural language
* **Learn from feedback**: Improve through evaluation and fine-tuning

## Part 1: Setting Up Your Environment

### Create an Azure AI Foundry Hub

The hub is your organizational workspace for AI projects:

1. Navigate to Azure portal
1. Search for "Azure AI Foundry" or "Azure AI Studio"
1. Click "Create" > "New hub"
1. Configure hub settings:
   * **Subscription**: Select your Azure subscription
   * **Resource group**: Create new or select existing
   * **Name**: Choose descriptive name (e.g., `ai-agents-hub`)
   * **Region**: Select region closest to users
   * **Storage**: Default storage account will be created
1. Click "Review + Create" then "Create"
1. Wait 2-3 minutes for deployment to complete

> **Note**: The hub provisions multiple Azure resources including storage, key vault, container
> registry, and application insights for comprehensive AI development.

### Create Your First Project

Projects organize agents, data, and experiments:

1. Open your newly created hub
1. Click "Create project" or "+ New project"
1. Enter project details:
   * **Project name**: Descriptive name (e.g., `customer-support-agent`)
   * **Description**: Purpose of this project
   * **Hub**: Verify correct hub is selected
1. Click "Create"
1. Project initializes in 30-60 seconds

### Configure Model Deployments

Deploy the models your agent will use:

1. In your project, navigate to "Deployments" section
1. Click "Create deployment"
1. Select model:
   * **Model family**: Azure OpenAI
   * **Model**: GPT-4o (recommended for agents)
   * **Version**: Latest stable version
1. Configure deployment:
   * **Deployment name**: `gpt-4o-deployment`
   * **Tokens per minute rate limit**: 30K (adjust based on needs)
   * **Content filter**: Default (moderate filtering)
1. Click "Deploy"
1. Repeat for embedding model:
   * **Model**: text-embedding-ada-002
   * **Deployment name**: `text-embedding-deployment`

> **Important**: Model deployments incur costs based on token usage. Start with lower rate limits
> during development and scale up for production.

## Part 2: Building Your First Agent

### Understanding Agent Architecture

Azure AI Foundry agents consist of:

#### Foundation Model

The core LLM that powers understanding and generation (e.g., GPT-4o, GPT-4, GPT-3.5).

#### System Instructions

Directives that define agent behavior, personality, and constraints.

#### Grounding Data

Knowledge sources the agent can search (documents, databases, APIs).

#### Tools and Functions

Actions the agent can perform (calculations, API calls, code execution).

#### Memory

Conversation history and context maintained across interactions.

### Create Agent in Playground

1. Navigate to "Agents" in left sidebar
1. Click "Create new agent"
1. Choose "Quick start" template
1. Configure basic settings:
   * **Agent name**: `SupportBot`
   * **Description**: Handles customer inquiries using knowledge base
   * **Model**: Select your deployed GPT-4o model

### Define System Instructions

System instructions guide your agent's behavior:

```text
You are SupportBot, a helpful customer service assistant for a technology company.

Your responsibilities:
1. Answer customer questions about products, services, and policies
2. Troubleshoot common technical issues
3. Guide customers to appropriate resources when needed

Guidelines:
- Be professional, friendly, and empathetic
- Provide clear, step-by-step instructions
- Always cite sources when referencing documentation
- If you don't know something, admit it and offer to escalate

Tone: Professional yet approachable
Response length: Concise but complete (2-4 sentences for simple queries)

Important boundaries:
- Do NOT make promises about refunds, shipping, or service credits
- Do NOT access or request customer payment information
- Do NOT provide advice outside documented policies
```

Enter this in the "System message" field in the agent configuration.

### Add Grounding Data

Connect knowledge sources your agent can search:

#### Upload Documents

1. Click "Add data" > "Upload files"
1. Select files to upload:
   * Product documentation (PDF, DOCX)
   * FAQ documents
   * Policy guides
   * Troubleshooting manuals
1. Configure indexing:
   * **Chunk size**: 1024 tokens (balance between context and precision)
   * **Chunk overlap**: 128 tokens (prevents information loss)
   * **Search type**: Hybrid (combines keyword and semantic search)
1. Click "Upload and process"
1. Wait for indexing to complete (varies by document size)

#### Connect to Azure AI Search

For larger document collections:

1. Click "Add data" > "Azure AI Search"
1. Select or create search service
1. Choose index:
   * Create new index from existing data
   * Or connect to pre-configured index
1. Configure retrieval:
   * **Top K**: 5 (number of relevant chunks to retrieve)
   * **Minimum score**: 0.3 (relevance threshold)
   * **Enable semantic ranking**: Yes (for better relevance)

### Configure Agent Tools

Enable built-in tools for extended capabilities:

#### Code Interpreter

Allows agent to write and execute Python code:

1. In agent settings, toggle "Code interpreter" to ON
1. Configure sandbox:
   * **Timeout**: 30 seconds
   * **Memory limit**: 512 MB
   * **Allowed packages**: pandas, numpy, matplotlib (data analysis essentials)

Use cases: Calculations, data analysis, chart generation

#### Function Calling

Define custom functions the agent can invoke:

```json
{
  "name": "check_order_status",
  "description": "Retrieves current status of a customer order",
  "parameters": {
    "type": "object",
    "properties": {
      "order_id": {
        "type": "string",
        "description": "The order ID to look up"
      }
    },
    "required": ["order_id"]
  }
}
```

1. Click "Add function"
1. Paste function schema above
1. Configure function endpoint:
   * **Type**: Azure Function, Logic App, or REST API
   * **URL**: Your function endpoint URL
   * **Authentication**: API key, managed identity, or OAuth

## Part 3: Testing Your Agent

### Use the Playground

The playground provides interactive testing:

1. Navigate to "Playground" tab in your agent
1. Start with simple queries:

```text
User: What are your business hours?
Agent: [Should search knowledge base and provide hours]
```

1. Test progressively complex scenarios:
   * Multi-turn conversations
   * Ambiguous queries requiring clarification
   * Questions outside agent knowledge
   * Requests requiring tool usage

### Evaluate Response Quality

For each test, assess:

#### Accuracy

* Does the answer match documented information?
* Are citations provided and correct?
* Are there any hallucinations or invented facts?

#### Relevance

* Does the response address the user's actual question?
* Is extraneous information included?
* Is the scope appropriate?

#### Tone and Style

* Does the response match system instructions?
* Is the language level appropriate?
* Is the personality consistent?

#### Completeness

* Are all aspects of the question addressed?
* Are next steps or follow-ups suggested?
* Is the response actionable?

### Debugging Common Issues

#### Agent Not Using Grounding Data

**Symptoms**: Generic responses without citations

**Solutions**:

1. Verify documents are indexed successfully
1. Check chunk size (too large may dilute relevance)
1. Adjust retrieval parameters (lower minimum score)
1. Add explicit instruction: "Always search the knowledge base before answering"

#### Incorrect Function Calls

**Symptoms**: Agent tries to call functions inappropriately

**Solutions**:

1. Improve function descriptions (be very specific about when to use)
1. Add examples in system instructions
1. Specify parameter formats clearly (e.g., "order_id format: ORD-XXXXXXXX")

#### Inconsistent Personality

**Symptoms**: Tone varies between responses

**Solutions**:

1. Make system instructions more prescriptive
1. Add specific examples of desired responses
1. Increase temperature parameter for creativity or decrease for consistency
1. Use few-shot examples in system message

## Part 4: Advanced Configuration

### Optimize Model Parameters

Fine-tune generation behavior:

#### Temperature (0.0 - 1.0)

* **Lower (0.0-0.3)**: Deterministic, factual, consistent
* **Medium (0.4-0.7)**: Balanced creativity and reliability
* **Higher (0.8-1.0)**: Creative, varied, unpredictable

Recommended: 0.3 for customer support, 0.7 for creative tasks

#### Top P (0.0 - 1.0)

Controls diversity of word choices:

* **0.9**: Good balance (recommended)
* **Lower**: More focused, repetitive
* **Higher**: More diverse, potentially off-topic

#### Max Response Length

* Customer support: 500 tokens (2-3 paragraphs)
* Detailed explanations: 1000 tokens
* Concise answers: 150 tokens

#### Frequency Penalty (0.0 - 2.0)

Reduces repetition:

* **0.0**: No penalty
* **0.3-0.5**: Moderate reduction (recommended)
* **1.0+**: Strong penalty, may affect coherence

### Implement Conversation Memory

Configure how context is maintained:

#### Session Management

1. Enable "Conversation history" in agent settings
1. Configure retention:
   * **Turns to remember**: 10 (balance between context and cost)
   * **Token limit**: 4000 tokens of history
   * **Summarization**: Auto-summarize after 8 turns

#### Context Compression

For long conversations:

1. Enable "Auto-summarization"
1. Set trigger: "After 2000 tokens of history"
1. Summary prompt: "Summarize key points and decisions from this conversation"

### Add Safety Guardrails

Implement content filtering and monitoring:

#### Content Filters

1. Navigate to "Content filters" in deployment settings
1. Configure severity thresholds:
   * **Hate**: Block high severity
   * **Sexual**: Block medium and high
   * **Violence**: Block high severity
   * **Self-harm**: Block all severities
1. Apply to both input and output

#### Custom Blocklists

1. Create blocklist of prohibited terms or topics
1. Add patterns to avoid:
   * Competitor names
   * Proprietary terminology
   * Sensitive internal terms
1. Configure action: Block or flag for review

#### Prompt Injection Protection

1. Enable "Jailbreak detection"
1. Set sensitivity: Medium (balance security and false positives)
1. Configure response: "I cannot process that request"

## Part 5: Connecting to Data Sources

### Azure AI Search Integration

For enterprise-scale document search:

#### Set Up Search Service

1. Create Azure AI Search service:
   * **Tier**: Basic (dev/test) or Standard (production)
   * **Replicas**: 1 (single instance) or more (high availability)
   * **Partitions**: 1 (up to 15GB) or more (larger datasets)

#### Create Index

1. In Azure AI Search, click "Import data"
1. Connect data source:
   * **Azure Blob Storage**: For documents
   * **Azure SQL Database**: For structured data
   * **Cosmos DB**: For NoSQL data
1. Configure indexer:
   * **Schedule**: Hourly, daily, or on-demand
   * **Field mappings**: Map source fields to searchable fields
   * **Skills**: Add OCR, entity extraction, or key phrase extraction
1. Create and run indexer

#### Connect to Agent

1. In agent configuration, add data source
1. Select "Azure AI Search"
1. Choose your search service and index
1. Configure query:
   * **Search fields**: title, content, metadata
   * **Filter**: Apply any constraints (e.g., category = 'public')
   * **Semantic ranking**: Enable for better relevance

### API Connections

Integrate with external systems:

#### REST API Tool

1. In agent tools, click "Add API connection"
1. Define API specification (OpenAPI/Swagger)
1. Configure authentication:
   * **API Key**: Store in Azure Key Vault
   * **OAuth 2.0**: Configure provider and scopes
   * **Managed Identity**: For Azure services
1. Map function to API endpoint

Example: Weather API

```json
{
  "name": "get_weather",
  "description": "Get current weather for a location",
  "method": "GET",
  "url": "https://api.weather.com/v1/current",
  "parameters": {
    "location": {
      "type": "string",
      "in": "query",
      "required": true
    },
    "units": {
      "type": "string",
      "in": "query",
      "default": "metric"
    }
  },
  "authentication": {
    "type": "apiKey",
    "key_name": "api_key",
    "in": "header"
  }
}
```

### Database Connectivity

Query structured data directly:

#### Azure SQL Database

1. Create Azure SQL connection in agent
1. Configure with managed identity (secure, no passwords)
1. Define allowed queries (prevent SQL injection):

```sql
-- Predefined query templates
SELECT status, expected_delivery
FROM orders
WHERE order_id = @order_id AND customer_id = @customer_id
```

1. Map to agent function:

```json
{
  "name": "get_order_info",
  "description": "Retrieve order details from database",
  "parameters": {
    "order_id": {"type": "string"},
    "customer_id": {"type": "string"}
  }
}
```

## Part 6: Deployment and Integration

### Deploy to Azure Container Apps

Create a production endpoint:

1. In your agent, click "Deploy"
1. Select deployment target: "Azure Container Apps"
1. Configure deployment:
   * **Name**: `supportbot-production`
   * **Region**: Same as hub for lower latency
   * **Instance count**: 1-3 (based on expected load)
   * **Authentication**: Require API key
1. Click "Deploy"
1. Wait 5-10 minutes for deployment
1. Note endpoint URL and API key

### Test Deployment

Use the provided endpoint:

```bash
curl -X POST https://your-agent.azurecontainerapps.io/chat \
  -H "Content-Type: application/json" \
  -H "api-key: YOUR_API_KEY" \
  -d '{
    "message": "What are your business hours?",
    "session_id": "test-session-123"
  }'
```

Response:

```json
{
  "message": "Our business hours are Monday-Friday, 9 AM to 5 PM EST...",
  "citations": [
    {
      "title": "Business Information",
      "url": "https://...",
      "chunk": "..."
    }
  ],
  "session_id": "test-session-123"
}
```

### Integrate with Applications

#### Web Application

JavaScript example:

```javascript
async function chatWithAgent(message, sessionId) {
  const response = await fetch('https://your-agent.azurecontainerapps.io/chat', {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
      'api-key': 'YOUR_API_KEY'
    },
    body: JSON.stringify({
      message: message,
      session_id: sessionId
    })
  });

  return await response.json();
}

// Usage
const result = await chatWithAgent('Help me reset my password', 'user-session-456');
console.log(result.message);
```

#### Teams Bot

Integrate with Microsoft Teams:

1. In deployment settings, enable "Teams integration"
1. Configure Teams app:
   * **App name**: SupportBot
   * **Description**: Internal support assistant
   * **Icon**: Upload bot avatar
1. Generate Teams manifest
1. Upload to Teams admin center
1. Publish to organization

#### Power Platform

Connect to Power Apps or Power Automate:

1. Create custom connector in Power Platform
1. Import OpenAPI spec from agent deployment
1. Use in Power Apps:
   * Add chat interface component
   * Connect to agent connector
   * Handle responses and display

## Part 7: Monitoring and Optimization

### Monitor Usage and Performance

#### Application Insights

Automatically enabled with deployment:

1. Navigate to Application Insights resource
1. View key metrics:
   * **Request rate**: Requests per minute
   * **Response time**: P50, P90, P95 latencies
   * **Failure rate**: 4xx and 5xx errors
   * **Token usage**: Prompt and completion tokens

#### Cost Tracking

1. In Azure portal, navigate to Cost Management
1. Filter by resource group
1. View breakdown:
   * **Model inference**: Token costs (largest component)
   * **Search queries**: AI Search costs
   * **Storage**: Document and index storage
   * **Compute**: Container Apps hosting

#### Usage Analytics

Track agent effectiveness:

1. In Azure AI Foundry, view "Analytics" tab
1. Review metrics:
   * **Total conversations**: Unique sessions
   * **Average turns per session**: Engagement indicator
   * **Resolution rate**: Successfully answered queries
   * **Escalation rate**: Human handoff frequency
   * **User satisfaction**: From feedback surveys

### Optimize Performance

#### Reduce Latency

##### Strategy 1: Cache Common Queries

1. Implement Redis cache for frequently asked questions
1. Set TTL based on content freshness needs
1. Cache hit rate target: >30%

##### Strategy 2: Optimize Retrieval

1. Reduce Top K from 5 to 3 (fewer documents to process)
1. Use semantic ranking (better relevance with fewer results)
1. Pre-filter irrelevant documents (category, date, etc.)

##### Strategy 3: Parallel Processing

1. Enable parallel function calls when independent
1. Async grounding data retrieval
1. Stream responses for perceived speed

#### Reduce Costs

##### Strategy 1: Right-Size Models

Consider GPT-3.5 Turbo for simple queries:

* 10x lower cost than GPT-4
* Sufficient for straightforward Q&A
* Use GPT-4 only when complexity requires it

##### Strategy 2: Optimize Token Usage

1. Limit conversation history to 5 turns
1. Use summarization for longer contexts
1. Reduce system message verbosity
1. Set max_tokens to minimum required

##### Strategy 3: Implement Tiering

Route queries based on complexity:

* Simple FAQ � GPT-3.5 Turbo
* Complex reasoning � GPT-4
* Automated classification or user selection

### Continuous Improvement

#### Evaluation Framework

Create test sets to measure quality:

1. Navigate to "Evaluation" in Azure AI Foundry
1. Create evaluation dataset:
   * 50-100 representative queries
   * Include edge cases and common errors
   * Add expected outputs or criteria
1. Run evaluation:
   * **Groundedness**: Are answers based on sources?
   * **Relevance**: Do answers address questions?
   * **Coherence**: Are responses well-structured?
   * **Fluency**: Is language natural?
1. Review results and iterate on configuration

#### A/B Testing

Test changes before full rollout:

1. Deploy candidate version alongside production
1. Route 10% of traffic to new version
1. Compare metrics:
   * Response quality scores
   * User satisfaction ratings
   * Performance metrics
   * Cost per conversation
1. Gradually increase traffic if metrics improve

#### Feedback Loop

Implement user feedback:

1. Add thumbs up/down to agent responses
1. Optional comment field for details
1. Store feedback in database
1. Weekly review:
   * Identify patterns in negative feedback
   * Update system instructions or grounding data
   * Adjust model parameters if needed

## Part 8: Best Practices

### Agent Design Principles

#### Clarity Over Cleverness

* Prioritize clear, direct answers over creative responses
* Use simple language unless technical audience requires otherwise
* Break complex information into digestible parts

#### Transparency

* Always cite sources for factual claims
* Acknowledge limitations honestly
* Explain reasoning when appropriate (especially for decisions)

#### Consistency

* Maintain personality throughout conversation
* Use consistent terminology
* Apply rules uniformly across all interactions

### Prompt Engineering Tips

#### Effective System Instructions

**Structure**:

1. Role and purpose (who/what the agent is)
1. Core responsibilities (what it does)
1. Guidelines (how it should behave)
1. Boundaries (what it should not do)

**Specificity**:

* Bad: "Be helpful"
* Good: "Provide step-by-step troubleshooting instructions with screenshots when available"

**Examples**:

Include few-shot examples for complex behaviors:

```text
Example interaction:

User: My app keeps crashing
Agent: I'll help you troubleshoot the crashing issue. To start, could you tell me:
1. Which device are you using? (iOS/Android/Desktop)
2. When did the crashes start?
3. What were you doing when it crashed?

This information will help me identify the issue more quickly.
```

### Grounding Data Best Practices

#### Document Preparation

Before indexing:

1. **Remove duplicates**: Avoid contradictory information
1. **Update outdated content**: Set review schedules
1. **Add metadata**: Author, date, category, version
1. **Optimize formatting**: Use headers, lists, tables
1. **Include keywords**: Improve searchability

#### Chunking Strategy

* **Size**: 512-1024 tokens per chunk (balance context and precision)
* **Overlap**: 10-20% overlap to prevent context loss
* **Method**: Respect document structure (don't split mid-paragraph)

#### Search Optimization

* **Hybrid search**: Combine keyword and semantic for best results
* **Semantic ranking**: Enable for improved relevance (worth the cost)
* **Metadata filtering**: Use categories, dates to narrow results
* **Boost factors**: Increase relevance of high-quality sources

### Security Considerations

#### Data Protection

* Store sensitive data in Azure Key Vault
* Use managed identities for service-to-service auth
* Enable encryption at rest and in transit
* Implement RBAC for resource access

#### Input Validation

* Sanitize user inputs before processing
* Reject malformed requests
* Implement rate limiting to prevent abuse
* Log all interactions for audit trail

#### Output Filtering

* Enable content filters on output
* Review generated responses in sensitive domains
* Implement custom filters for domain-specific risks
* Monitor for data leakage (PII, credentials)

## Part 9: Troubleshooting Guide

### Agent Not Responding

**Symptoms**: Timeout errors or no response

**Solutions**:

1. Check deployment status in Azure portal
1. Verify endpoint URL is correct
1. Confirm API key is valid and not expired
1. Review Application Insights for errors
1. Check model deployment quotas (may be exhausted)
1. Increase timeout in client application

### Poor Quality Responses

**Symptoms**: Irrelevant, incorrect, or off-topic answers

**Solutions**:

1. Review and improve system instructions
1. Verify grounding data is properly indexed
1. Adjust retrieval parameters (Top K, minimum score)
1. Test with different temperature settings
1. Add more specific examples to system message
1. Consider switching to more capable model (GPT-4)

### High Costs

**Symptoms**: Unexpectedly high Azure bills

**Solutions**:

1. Review token usage in Application Insights
1. Implement caching for common queries
1. Reduce conversation history retention
1. Optimize system message length
1. Set max_tokens limits on responses
1. Consider cheaper models for simple queries
1. Implement rate limiting per user

### Slow Response Times

**Symptoms**: Latency >5 seconds consistently

**Solutions**:

1. Reduce grounding data retrieval count
1. Disable unnecessary tools or functions
1. Use faster embedding models
1. Enable response streaming
1. Cache frequent queries
1. Co-locate resources in same region
1. Increase instance count for parallel processing

### Function Calling Errors

**Symptoms**: Agent fails to call functions or calls incorrectly

**Solutions**:

1. Improve function descriptions (be very specific)
1. Add examples of when/how to use function
1. Validate function schema matches actual API
1. Check function endpoint is accessible
1. Review authentication configuration
1. Add error handling in function code
1. Test functions independently before agent integration

## Summary

You've learned to build production-ready AI agents with Azure AI Foundry. Key takeaways:

* **Setup**: Hub, project, and model deployment structure
* **Agent building**: System instructions, grounding data, and tools
* **Testing**: Playground testing and quality evaluation
* **Deployment**: Container Apps deployment and API integration
* **Monitoring**: Usage analytics, cost tracking, and optimization
* **Best practices**: Design principles, security, and troubleshooting

## Next Steps

### Expand Your Skills

1. Experiment with multi-agent orchestration
1. Implement custom skills with Azure Functions
1. Explore fine-tuning for domain-specific tasks
1. Build evaluation pipelines for continuous testing
1. Learn Prompt Flow for complex agent workflows

### Production Readiness

1. Implement comprehensive logging and alerting
1. Set up CI/CD pipelines for agent deployments
1. Create disaster recovery and backup strategies
1. Develop escalation workflows to human agents
1. Conduct security and compliance reviews

### Community and Resources

* Azure AI Foundry documentation and samples
* Azure OpenAI service best practices
* Responsible AI guidelines and tools
* Community forums and user groups
* Regular webinars and training sessions

---

**Feedback**: Share your experience building with Azure AI Foundry to help improve this tutorial.
