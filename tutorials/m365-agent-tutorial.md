# Build and Deploy M365 Copilot Agents from the Chat Experience

## Overview

M365 Copilot agents transform how users interact with organizational data and automate
workflows through natural language conversations. This tutorial guides you through creating,
configuring, and deploying intelligent agents that integrate seamlessly with the M365 ecosystem,
accessible directly from the Copilot chat interface.

**Estimated time to complete**: 45-60 minutes

## Prerequisites

Before you begin, ensure you have:

* M365 Copilot license with appropriate permissions
* Access to M365 admin center or delegated agent creation permissions
* Basic understanding of generative AI concepts and prompt engineering
* Familiarity with M365 applications (SharePoint, Teams, OneDrive)
* Optional: Power Platform environment access for advanced scenarios

## Learning Objectives

By the end of this tutorial, you'll be able to:

* Understand the M365 Copilot agent architecture and capabilities
* Create and configure agents through declarative approaches
* Integrate organizational knowledge sources for grounded responses
* Test and iterate on agent behaviors using the chat interface
* Implement security and governance best practices
* Deploy agents to users and monitor their effectiveness

## What are M365 Copilot Agents

M365 Copilot agents are AI-powered assistants built on large language models that:

* **Understand context**: Interpret user intent from natural language
* **Access organizational data**: Query across SharePoint, OneDrive, Teams, and other M365 sources
* **Maintain conversation flow**: Remember context throughout multi-turn dialogues
* **Execute actions**: Perform tasks like summarizing documents, drafting content, or querying data
* **Learn from feedback**: Improve responses based on user interactions

### Agent Types

#### Declarative Agents

Simple agents defined through JSON configuration files specifying instructions, capabilities,
and knowledge sources. Best for focused use cases with well-defined scope.

#### Plugin-based Agents

Extended agents that integrate external APIs, custom connectors, or Power Platform components.
Enable complex workflows and enterprise system integration.

#### Custom Agents

Fully custom solutions using Copilot Studio or development frameworks for specialized
enterprise scenarios requiring advanced orchestration.

## Part 1: Understanding the Chat Experience

### Accessing M365 Copilot

The M365 Copilot chat interface serves as the primary interaction point:

1. Navigate to the Copilot icon in your M365 app bar
1. Click to open the unified chat experience
1. The interface provides access to base Copilot and organizational agents

### Chat Interface Components

#### Prompt Input

* Natural language text input with multimodal support (images, files)
* @ mentions for invoking specific agents or referencing content
* Suggested prompts based on context and recent activities

#### Response Panel

* Structured responses with citations and source references
* Interactive elements (buttons, cards, follow-up suggestions)
* Feedback mechanisms for response quality

#### Agent Selector

* Dropdown or sidebar showing available agents
* Default Copilot plus custom organizational agents
* Contextual agent recommendations based on current activity

## Part 2: Creating Your First Declarative Agent

### Agent Creation Methods

Microsoft 365 Copilot offers multiple ways to create declarative agents:

* **Agent Builder in Chat**: Create agents directly from Microsoft 365 Copilot chat using natural
  language or manual configuration (available as of 2024)
* **Microsoft 365 Agents Toolkit**: Use Visual Studio Code with TypeScript/TypeSpec for developers
* **Manual manifest creation**: Define JSON configuration files for advanced customization

This tutorial focuses on the declarative manifest approach, which works across all creation methods.

### Step 1: Define Agent Purpose and Scope

Before building, clearly define:

* **Primary use case**: What problem does this agent solve?
* **Target users**: Who will interact with this agent?
* **Data sources**: What information does the agent need?
* **Actions**: What can users accomplish through this agent?

#### Example Scenario

Create an "IT Support Assistant" agent that helps employees troubleshoot common technical
issues using internal knowledge base articles and support documentation.

### Step 2: Create the Agent Manifest

Declarative agents use a manifest file defining behavior and capabilities. The current schema
version is 1.5 (as of December 2024), which includes additional capabilities like Meetings,
GraphicArt, CodeInterpreter, and more. This example uses version 1.0 for simplicity, but you can
use newer versions to access advanced features.

```json
{
  "version": "v1.0",
  "name": "IT Support Assistant",
  "description": "Helps resolve common IT issues using company knowledge base",
  "instructions": "You are an IT support assistant for the organization. Help users troubleshoot common technical issues using the knowledge base. Always be polite, clear, and provide step-by-step guidance. If you cannot resolve an issue, guide users to submit a support ticket with relevant details.",
  "capabilities": [
    {
      "name": "OneDriveAndSharePoint",
      "items_by_url": [
        {
          "url": "https://yourorg.sharepoint.com/sites/IT-Support"
        }
      ]
    }
  ],
  "conversation_starters": [
    {
      "text": "I can't connect to the VPN"
    },
    {
      "text": "How do I reset my password?"
    },
    {
      "text": "My email isn't syncing"
    },
    {
      "text": "I need help with printer setup"
    }
  ]
}
```

#### Available Capabilities (Schema 1.5)

The latest schema version (1.5) supports these capabilities in the `capabilities` array:

* **OneDriveAndSharePoint**: Access files and sites (shown in example above)
* **WebSearch**: Enable web search with optional site restrictions
* **GraphConnectors**: Connect to Microsoft Graph connectors for external data
* **GraphicArt**: Generate images and art based on text input
* **CodeInterpreter**: Generate and execute Python code for data analysis
* **Dataverse**: Access Power Platform Dataverse tables
* **TeamsMessages**: Search Teams messages
* **Email**: Search email with optional shared mailbox access
* **People**: Search for people in the organization
* **Meetings**: Search meetings in the organization
* **ScenarioModels**: Use task-specific AI models

### Step 3: Configure Knowledge Sources

Ground your agent with organizational data:

#### SharePoint Sites

1. Navigate to agent configuration settings
1. Select "Add knowledge source" > "SharePoint site"
1. Enter site URL: `https://yourorg.sharepoint.com/sites/IT-Support`
1. Configure permissions: Use agent identity or delegated permissions
1. Set refresh schedule: Real-time, hourly, or daily

#### OneDrive Folders

1. Select "Add knowledge source" > "OneDrive location"
1. Browse to folder: `/IT Documentation/Support Guides`
1. Include subfolders: Enable for hierarchical content
1. Filter file types: .docx, .pdf, .xlsx recommended

#### Teams Channels

1. Select "Add knowledge source" > "Teams channel"
1. Choose team and channel: IT Support > Common Issues
1. Include conversations: Enable for community knowledge
1. Date range: Last 12 months recommended

> **Important**: Only add sources containing information you want the agent to access.
> Overly broad sources can dilute response relevance and increase processing time.

### Step 4: Craft Effective Instructions

Instructions guide agent behavior and personality:

#### Best Practices

* **Be specific**: Define exact behaviors and response patterns
* **Set boundaries**: Clearly state what the agent should NOT do
* **Include examples**: Show desired response formats
* **Define tone**: Specify professional, friendly, technical, etc.
* **Handle edge cases**: Address scenarios outside agent scope

#### Example Instructions Template

```text
You are [ROLE] for [ORGANIZATION/DEPARTMENT]. Your primary responsibility is to
[PRIMARY FUNCTION].

When responding:
1. [BEHAVIOR 1]: Always cite sources using [citation format]
2. [BEHAVIOR 2]: Provide step-by-step instructions for procedures
3. [BEHAVIOR 3]: Ask clarifying questions when user intent is unclear

You have access to:
- [DATA SOURCE 1]: Use for [specific purpose]
- [DATA SOURCE 2]: Reference when [specific scenario]

You should NOT:
- [LIMITATION 1]: Make promises about service delivery times
- [LIMITATION 2]: Override existing policies or procedures
- [LIMITATION 3]: Access or request sensitive credential information

When you cannot help, direct users to [ESCALATION PATH] with [REQUIRED INFORMATION].
```

### Step 5: Test in Chat Experience

1. Save your agent configuration
1. Wait for deployment (typically 5-15 minutes)
1. Open M365 Copilot chat interface
1. Select your agent from the agent picker
1. Start with conversation starters to validate basic functionality

#### Testing Checklist

*  Agent responds to all conversation starters appropriately
*  Knowledge sources are accessed and cited correctly
*  Instructions are followed consistently
*  Edge cases are handled gracefully
*  Response time is acceptable (typically < 10 seconds)

## Part 3: Advanced Agent Configuration

### Multimodal Interactions

Enable your agent to process images and documents:

#### Image Analysis

Users can upload screenshots of error messages:

```text
User: "I'm getting this error message" [uploads screenshot]
Agent: [Analyzes image] "This is a certificate validation error. Here's how to resolve it..."
```

#### Document Processing

Enable file attachment processing:

1. In agent settings, enable "Accept file uploads"
1. Specify allowed file types: .pdf, .docx, .xlsx, .pptx
1. Set maximum file size: 10MB recommended
1. Configure: "Process uploaded files for context and analysis"

### Conversation Flow Design

Structure multi-turn conversations effectively:

#### Context Retention

Agents automatically maintain conversation history. Design for progressive disclosure:

```text
User: "How do I set up email on my phone?"
Agent: "I can help with that. Which phone operating system are you using?"
User: "iOS"
Agent: "Great! Are you setting up for the first time, or troubleshooting an existing setup?"
User: "First time"
Agent: [Provides iOS-specific first-time setup instructions]
```

#### Follow-up Suggestions

Implement contextual next steps using conversation starters:

```json
{
  "conversation_starters": [
    {
      "title": "Test my connection",
      "text": "Walk me through testing my email connection"
    },
    {
      "title": "Set up calendar sync",
      "text": "How do I sync my calendar on this device?"
    },
    {
      "title": "Troubleshoot issues",
      "text": "My email setup isn't working correctly"
    }
  ]
}
```

### Integration with Actions

Extend agents beyond information retrieval:

#### Power Automate Flows

1. Create a Power Automate flow triggered by "When Copilot action is invoked"
1. Define input parameters (e.g., ticket subject, priority, description)
1. Add your workflow logic (create ticket, send notifications, update databases)
1. Publish flow and create an API plugin manifest describing the action
1. Add action reference to agent manifest:

```json
{
  "actions": [
    {
      "id": "create-support-ticket",
      "file": "create-ticket-plugin.json"
    }
  ]
}
```

Note: The detailed parameters and descriptions are defined in the plugin manifest file, which
follows the OpenAPI specification format.

#### Graph API Connectors

Connect to M365 services programmatically:

* Read calendar availability and schedule meetings
* Search for people with specific skills or roles
* Query organizational hierarchy and reporting structures
* Access project plans and task assignments

## Part 4: Prompt Engineering for Agents

### Crafting Effective User Prompts

Train users on optimal prompting techniques:

#### Specificity

* L Poor: "Email problem"
*  Better: "I can't send emails with attachments larger than 5MB"

#### Context

* L Poor: "It's not working"
*  Better: "After the recent update, I'm unable to access shared files in Teams on my mobile device"

#### Intent

* L Poor: "VPN information"
*  Better: "What are the step-by-step instructions to configure VPN on Windows 11?"

### System Prompt Optimization

Refine agent instructions through iteration:

**Version 1**: Basic instruction

```text
Help users with IT support issues.
```

**Version 2**: Structured approach

```text
You are an IT support agent. Answer technical questions using the knowledge base.
```

**Version 3**: Comprehensive guidance

```text
You are an IT Support Assistant helping employees resolve technical issues efficiently.

Response framework:
1. Acknowledge the issue empathetically
2. Ask clarifying questions if details are insufficient
3. Provide step-by-step solutions with clear actions
4. Reference specific knowledge base articles
5. Verify resolution or escalate appropriately

Always:
- Use simple, non-technical language unless user demonstrates technical expertise
- Provide visual descriptions of UI elements ("Click the gear icon in the top-right")
- Include time estimates for resolution steps
- Cite sources with article titles and links

Never:
- Request passwords or sensitive credentials
- Make promises about resolution timeframes
- Bypass security policies or procedures
- Provide solutions outside your knowledge base scope
```

## Part 5: Security and Governance

### Permissions and Access Control

#### Agent-level Permissions

Configure who can discover and use agents:

* **Everyone**: All organizational users (public agents)
* **Specific groups**: Azure AD security groups or M365 groups
* **Departments**: Users within specific organizational units
* **Custom policies**: Conditional access based on rules

#### Data Access Permissions

Agents respect Microsoft Entra ID (formerly Azure AD) permissions and Microsoft Purview policies:

* **User context (default)**: Agent accesses only what the user can access, using the same
  underlying controls as other Microsoft 365 services
* **Managed identity**: For autonomous scenarios, agents can run under a managed identity or
  service principal
* **Role-based access control (RBAC)**: Administrators can restrict agent creation and usage
  through the Copilot Control System

When you share an agent, users might not have access to all underlying knowledge sources. The
agent automatically respects each user's permissions, so if a user doesn't have access to a
knowledge source, the agent won't include that content in responses.

> **Security Note**: Always use the principle of least privilege. Microsoft 365 Copilot only
> surfaces organizational data to which individual users have at least view permissions.

### Content Filtering and Safety

#### Built-in Protections

* Harmful content filtering (hate speech, violence, self-harm)
* Jailbreak attempt detection and blocking
* Sensitive data pattern recognition (SSNs, credit cards)
* Prompt injection prevention

#### Custom Policies

Implement organizational guardrails through your agent instructions and Microsoft Purview:

* **Instructions-based boundaries**: Define blocked topics directly in agent instructions
  * Example: "Do not discuss executive compensation, merger plans, or layoff information"
* **Required disclaimers**: Add disclaimers using the optional `disclaimer` property in the manifest
* **Microsoft Purview policies**: Use Data Loss Prevention and Insider Risk Management to detect
  and prevent sensitive data exposure
* **Audit logging**: All agent conversations are automatically logged in the Microsoft 365
  compliance center for eDiscovery

### Compliance and Auditing

#### Conversation Logging

All agent interactions are logged for compliance:

* User queries and agent responses stored in compliance center
* Retention policies apply based on organizational settings
* eDiscovery accessible for legal and audit requirements
* Data residency honors organizational tenant location

#### Usage Analytics

Monitor agent effectiveness:

1. Navigate to Copilot admin dashboard
1. Select "Agent analytics" for your agent
1. Review metrics:
   * Total conversations and unique users
   * Average conversation length
   * User satisfaction ratings
   * Knowledge source utilization
   * Common queries and gaps

## Part 6: Deployment and Change Management

### Phased Rollout Strategy

#### Phase 1: Pilot Group (Weeks 1-2)

* Deploy to 10-20 enthusiastic early adopters
* Gather detailed feedback on functionality
* Identify gaps in knowledge sources
* Refine instructions based on real usage

#### Phase 2: Department Rollout (Weeks 3-4)

* Expand to full target department (50-200 users)
* Conduct brief training sessions
* Monitor usage patterns and adoption rates
* Address common misconceptions or barriers

#### Phase 3: Organization-Wide (Week 5+)

* Release to all eligible users
* Announce through multiple channels
* Provide self-service learning resources
* Establish support channels for user questions

### User Enablement

#### Communication Plan

* **Announcement email**: Purpose, benefits, how to access
* **Quick start guide**: One-page visual walkthrough
* **Video demonstration**: 2-3 minute screencast showing key scenarios
* **Office hours**: Weekly drop-in sessions for first month
* **Champions network**: Identify power users to peer-support

#### Training Resources

Create role-specific guidance:

* **End users**: How to write effective prompts, interpretation of responses
* **Content owners**: Maintaining quality knowledge sources
* **Administrators**: Agent management, monitoring, troubleshooting

### Monitoring and Optimization

#### Performance Metrics

Track key indicators:

* **Adoption**: % of target users engaging weekly
* **Engagement**: Average queries per user per week
* **Satisfaction**: Thumbs up/down ratings on responses
* **Deflection**: % of issues resolved without human escalation
* **Response quality**: Citation accuracy, hallucination rate

#### Continuous Improvement

Establish feedback loops:

1. **Weekly**: Review user feedback and low-rated responses
1. **Bi-weekly**: Analyze common query patterns for knowledge gaps
1. **Monthly**: Update instructions based on learnings
1. **Quarterly**: Reassess agent scope and knowledge sources

## Part 7: Common Patterns and Use Cases

### Knowledge Management Agent

**Purpose**: Answer questions using organizational documentation

**Configuration**:

* Knowledge sources: SharePoint sites, Wiki, Confluence
* Capabilities: Search, summarization, citation
* Key instruction: Always cite source documents

**Example Interactions**:

```text
User: "What's our expense reimbursement policy for client meals?"
Agent: "According to the Employee Handbook (updated March 2024), client meal
       reimbursements are limited to $75 per person. You'll need itemized receipts
       and documentation of attendees. Submit through the expense system within
       30 days. [Link to full policy]"
```

### Onboarding Assistant Agent

**Purpose**: Guide new employees through onboarding tasks

**Configuration**:

* Knowledge sources: HR portal, IT setup guides, benefits information
* Actions: Schedule meetings, create accounts, submit forms
* Key instruction: Use checklist format, track completion

**Example Interactions**:

```text
User: "I'm starting next Monday. What do I need to do?"
Agent: "Welcome! Here's your onboarding checklist:

       Week 1 - Essential Setup:
        Complete IT orientation (Day 1, 9am - link to invite)
        Set up workstation and accounts (Day 1 afternoon)
        Review security policies (Complete by Day 2)
        Submit benefits enrollment (Due by Day 5)

       Would you like details on any of these items?"
```

### Data Analysis Agent

**Purpose**: Query and visualize organizational data

**Configuration**:

* Knowledge sources: Data warehouses, Power BI datasets, Excel reports
* Capabilities: SQL generation, chart creation, trend analysis
* Key instruction: Explain methodology, show confidence levels

**Example Interactions**:

```text
User: "What were our top-performing products last quarter?"
Agent: "Based on Q3 2024 sales data, here are the top 5 products:

       1. Product Alpha: $2.4M revenue (�15% YoY)
       2. Product Beta: $1.9M revenue (�8% YoY)
       3. Product Gamma: $1.7M revenue (�3% YoY)
       4. Product Delta: $1.5M revenue (�22% YoY)
       5. Product Epsilon: $1.3M revenue (�5% YoY)

       Notable: Product Delta shows strongest growth trajectory.
       [View detailed analysis]"
```

### Meeting Preparation Agent

**Purpose**: Summarize context before meetings

**Configuration**:

* Knowledge sources: Calendar, emails, Teams chats, documents
* Capabilities: Summarization, participant analysis, action item tracking
* Key instruction: Prioritize recent, relevant information

**Example Interactions**:

```text
User: "Prepare me for my 2pm Project Phoenix meeting"
Agent: "Project Phoenix Status Meeting - Today 2:00pm (30 min)

       Key Context:
       " Last meeting: 2 weeks ago - decided on vendor selection
       " Recent activity: Contract negotiations in final stage
       " Open action items: You owe cost-benefit analysis (due today)

       Participants (8):
       " Sarah Chen (Project Lead) - Likely to discuss timeline
       " Mike Rodriguez (Finance) - Needs budget approval clarity

       Suggested prep: Review vendor contract draft in Teams Files

       Anything specific you'd like me to summarize?"
```

## Part 8: Troubleshooting Common Issues

### Agent Not Responding

**Symptoms**: Agent selected but produces no output or errors

**Resolution Steps**:

1. Verify agent deployment status in admin center
1. Check user permissions for agent access
1. Confirm knowledge sources are indexed and accessible
1. Review service health dashboard for outages
1. Clear browser cache and retry
1. Test with different user account to isolate issue

### Irrelevant or Inaccurate Responses

**Symptoms**: Agent provides off-topic or factually incorrect answers

**Resolution Steps**:

1. Review knowledge sources for content gaps or outdated information
1. Refine agent instructions to be more specific about scope
1. Add negative examples showing what NOT to do
1. Enable stricter citation requirements
1. Implement topic boundaries in configuration
1. Consider reducing knowledge source breadth

### Knowledge Source Not Being Used

**Symptoms**: Agent doesn't reference specific SharePoint site or document

**Resolution Steps**:

1. Verify site URL and permissions are correctly configured
1. Check if content has been indexed (can take 24-48 hours initially)
1. Confirm file types are supported (.docx, .pdf, .xlsx work best)
1. Test direct search in SharePoint to verify content is discoverable
1. Reduce knowledge source count if too many competing sources
1. Add explicit instruction to reference specific source types

### Slow Response Times

**Symptoms**: Agent takes >15 seconds to respond consistently

**Resolution Steps**:

1. Reduce number of knowledge sources (>10 sources can impact performance)
1. Narrow scope of large SharePoint sites to specific document libraries
1. Disable web search if not needed
1. Simplify complex instructions
1. Check for network latency issues
1. Review service health for regional performance issues

### Users Can't Find the Agent

**Symptoms**: Target users don't see agent in their Copilot interface

**Resolution Steps**:

1. Verify agent is published (not in draft status)
1. Check access permissions match user's group memberships
1. Confirm users have M365 Copilot licenses
1. Wait 15-30 minutes after deployment for propagation
1. Ask users to refresh browser or restart M365 app
1. Verify agent visibility settings aren't set to "Hidden"

## Part 9: Best Practices and Recommendations

### Design Principles

#### Focus and Specificity

* Create targeted agents for specific use cases rather than generalist agents
* One agent handling IT support is better than one handling "all employee questions"
* Users prefer specialized expertise over broad but shallow capability

#### Progressive Disclosure

* Start conversations with clarifying questions
* Present information in digestible chunks
* Offer pathways to explore deeper based on user needs
* Don't overwhelm with all possible information upfront

#### Transparency

* Always cite sources for factual claims
* Acknowledge uncertainty when appropriate
* Distinguish between data-backed answers and general guidance
* Make limitations clear to users

### Content Strategy

#### Knowledge Source Hygiene

* Regularly audit connected sources for outdated content
* Establish ownership and update schedules for key documents
* Use version control and clear file naming conventions
* Archive obsolete information rather than deleting (preserves history)

#### Documentation Quality

* Agents perform best with clear, well-structured source documents
* Use consistent formatting, headings, and terminology
* Front-load key information (summaries at the top)
* Include metadata (dates, owners, review cycles)

### User Experience

#### Conversation Starters

* Feature 4-6 high-value conversation starters
* Use natural language that matches how users think about problems
* Test starters with representative users before deployment
* Update based on actual query patterns over time

#### Response Formatting

* Use numbered lists for sequential steps
* Use bullet points for options or features
* Include visual cues (, L, �) sparingly for emphasis
* Break long responses into sections with clear headers

#### Feedback Mechanisms

* Encourage users to rate responses (thumbs up/down)
* Provide easy escalation path ("Talk to a person")
* Include feedback link in agent profile
* Act on feedback patterns monthly

### Governance

#### Change Management

* Document all agent configuration changes
* Test changes in non-production environment when possible
* Communicate significant capability changes to users
* Maintain rollback plan for problematic updates

#### Access Reviews

* Quarterly review of agent access permissions
* Verify group memberships still align with intended audience
* Audit knowledge source permissions for appropriate scope
* Review and remove unused or underperforming agents

#### Compliance Alignment

* Coordinate with legal/compliance teams before deployment
* Document data handling and retention practices
* Ensure alignment with existing AI governance policies
* Prepare for audits with clear documentation trail

## Summary

You've learned to build M365 Copilot agents that deliver value through natural language
interaction. Key takeaways:

* **Declarative agents** provide rapid development with JSON configuration
* **Knowledge grounding** ensures accurate, source-backed responses
* **Effective instructions** shape agent behavior and personality
* **Phased deployment** minimizes risk and maximizes adoption
* **Continuous monitoring** drives ongoing optimization

## Next Steps

### Immediate Actions

1. Identify a high-impact use case in your organization
1. Audit existing documentation for agent knowledge sources
1. Draft initial agent manifest and instructions
1. Recruit pilot group of 10-20 users
1. Deploy, test, gather feedback, iterate

### Further Learning

* Explore Power Platform integration for advanced workflows
* Study prompt engineering techniques for specialized domains
* Learn about custom plugin development for unique requirements
* Join community forums to share learnings and get support

### Advanced Topics

* Multi-agent orchestration patterns
* Custom model fine-tuning for domain expertise
* Enterprise-scale agent governance frameworks
* Measuring ROI and business impact of AI agents

## Additional Resources

### Official Microsoft Documentation

* [Declarative Agents Overview](https://learn.microsoft.com/en-us/microsoft-365-copilot/extensibility/overview-declarative-agent) - Microsoft Learn
* [Declarative Agent Schema 1.5](https://learn.microsoft.com/en-us/microsoft-365-copilot/extensibility/declarative-agent-manifest-1.5) - Latest schema reference
* [Agent Builder in Microsoft 365 Copilot](https://learn.microsoft.com/en-us/microsoft-365-copilot/extensibility/copilot-studio-lite) - Create agents from chat
* [Add Knowledge Sources](https://learn.microsoft.com/en-us/microsoft-365-copilot/extensibility/knowledge-sources) - SharePoint, OneDrive configuration
* [Write Effective Instructions](https://learn.microsoft.com/en-us/microsoft-365-copilot/extensibility/declarative-agent-instructions) - Prompt engineering guide
* [Security and Governance](https://learn.microsoft.com/en-us/copilot/microsoft-365/copilot-control-system/security-governance) - Copilot Control System
* [Agent Flows Overview](https://learn.microsoft.com/en-us/microsoft-copilot-studio/flows-overview) - Power Automate integration

### Related Topics

* M365 Copilot adoption framework and organizational change management
* SharePoint content organization for AI optimization
* Microsoft Purview policies for AI workloads
* Community samples and templates repository

---

**Feedback**: Help us improve this tutorial by sharing your experience and suggestions.
