# Basic Copilot Studio Agent Tutorial

This tutorial guides you through creating your first agent in Microsoft Copilot Studio using
the conversational agent creation experience.

## Prerequisites

Before starting, ensure you have:

* **License**: Copilot Studio subscription (standalone or included with Microsoft 365)
  * Trial users can build and test agents but cannot publish them
  * 60-day free trial available if you don't have a license
* **Access**: Permissions to create agents in your Microsoft environment
* **Browser**: Modern web browser with your preferred language configured

## What is a Copilot Studio Agent?

An agent is an AI companion that handles conversations and tasks. Agents can answer questions,
resolve issues, and take autonomous actions based on instructions and context. They use
generative AI orchestration to choose the best tools, knowledge, and topics to respond to users.

## Step 1: Create a New Agent

1. Sign in to [Microsoft Copilot Studio](https://copilotstudio.microsoft.com)
1. Select **Create** from the left navigation pane
1. Choose **New agent** to open the conversational creation interface
1. You'll see a two-pane interface:
   * **Left pane**: Configure your agent (Describe and Configure tabs)
   * **Right pane**: Test your agent in real-time

## Step 2: Describe Your Agent

Use the **Describe** tab to build your agent conversationally:

1. **Name**: Enter a descriptive name (maximum 30 characters)
1. **Description**: Briefly explain your agent's purpose (up to 1,000 characters)
1. **Instructions**: Define what your agent should do (up to 8,000 characters)
   * Use conversational language and active voice
   * Be specific about goals and desired behavior
   * Example: "Help employees find HR policies and answer benefits questions"
1. **Tone**: Specify personality traits (e.g., "professional and helpful")

As you provide information, Copilot refines your agent's configuration automatically.

## Step 3: Add Knowledge Sources

Knowledge sources ground your agent in enterprise data:

1. In the left pane, locate the **Knowledge** section
1. Select **Add knowledge** to choose from:
   * **Public websites**: Enter URLs for public documentation
   * **SharePoint**: Connect to SharePoint sites or document libraries
   * **OneDrive**: Link to OneDrive files and folders
   * **Dataverse**: Connect to Dynamics 365 or Power Apps tables
   * **File upload**: Upload documents (PDF, DOCX, PPTX up to 512 MB)
   * **Azure AI Search**: Connect to indexed external data
1. Add up to 5 different knowledge sources per agent
1. Configure authentication if using SharePoint or Dataverse (Microsoft Entra ID preconfigured)

**Note**: Enable "Tenant graph grounding with semantic search" for SharePoint sources to
improve knowledge retrieval and response quality.

## Step 4: Configure Topics (Optional)

While agents use generative AI by default, you can add custom topics for specific scenarios:

1. Switch to the **Configure** tab in the left pane
1. Navigate to **Topics**
1. Select **Add a topic** to create structured conversation flows
1. Define trigger phrases and response nodes
1. Use Copilot to generate topic content with AI assistance

**Tip**: System topics (greeting, escalation, end conversation) are built-in and handle
essential behaviors automatically.

## Step 5: Test Your Agent

1. Use the **Test agent** pane on the right side of the screen
1. Type questions or requests as an end user would
1. Evaluate responses for accuracy and tone
1. Return to the Describe or Configure tabs to refine your agent
1. Iterate between testing and configuration until satisfied

**Note**: Test chat messages don't count toward billed usage.

## Step 6: Publish Your Agent

1. Click **Publish** in the top-right corner
1. Review your agent's configuration summary
1. Select a publishing option:
   * **Demo website**: Share a URL for stakeholder testing
   * **Microsoft Teams**: Deploy to Teams channels
   * **Microsoft 365 Copilot**: Extend M365 Copilot with your agent
   * **Other channels**: Web, mobile apps, Facebook Messenger
1. Confirm and publish (applies to all configured channels)
1. Share the demo site link or channel access with users

**Note**: Publishing requires a paid Copilot Studio license. Trial users cannot publish agents.

## Summary

You've successfully created, configured, and published a Copilot Studio agent! Your agent can:

* Respond to user queries using generative AI
* Ground answers in enterprise knowledge sources
* Handle conversations with custom topics and system behaviors
* Deploy to multiple channels without requiring user licenses

Continue exploring advanced features like actions, plugins, and generative orchestration to
enhance your agent's capabilities.

## Additional Resources

* [Official Copilot Studio Documentation](https://learn.microsoft.com/en-us/microsoft-copilot-studio/)
* [Knowledge Sources Overview](https://learn.microsoft.com/en-us/microsoft-copilot-studio/knowledge-copilot-studio)
* [Topics and Orchestration](https://learn.microsoft.com/en-us/microsoft-copilot-studio/guidance/topics-overview)
* [Publishing and Channels](https://learn.microsoft.com/en-us/microsoft-copilot-studio/publication-fundamentals-publish-channels)
