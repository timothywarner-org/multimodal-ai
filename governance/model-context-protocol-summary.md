# Model Context Protocol (MCP) Summary

## Overview

The **Model Context Protocol (MCP)** is an open-source standard for connecting
AI applications to external systems. Think of MCP like a USB-C port for AI
applications—just as USB-C provides a standardized way to connect electronic
devices, MCP provides a standardized way to connect AI applications to external
systems.

Using MCP, AI applications like Claude or ChatGPT can connect to:

* **Data sources**: Local files, databases, APIs
* **Tools**: Search engines, calculators, specialized functions
* **Workflows**: Specialized prompts, interaction templates

## Key Benefits

### For Developers

MCP reduces development time and complexity when building or integrating with AI
applications and agents.

### For AI Applications

MCP provides access to an ecosystem of data sources, tools, and apps which
enhance capabilities and improve the end-user experience.

### For End-Users

MCP results in more capable AI applications and agents which can access user
data and take actions on their behalf when necessary.

## Real-World Use Cases

* **Personalization**: Agents can access Google Calendar and Notion to act as
  more personalized AI assistants
* **Design-to-Code**: Claude Code can generate an entire web app using a Figma
  design
* **Enterprise Integration**: Enterprise chatbots can connect to multiple
  databases across an organization, empowering users to analyze data using chat
* **3D Design & Manufacturing**: AI models can create 3D designs on Blender and
  print them using a 3D printer

## Architecture

### Participants

MCP follows a **client-server architecture** with three key participants:

1. **MCP Host**: The AI application that coordinates and manages MCP clients
   (e.g., VS Code, Claude Desktop, Claude Code)

1. **MCP Client**: A component that maintains a connection to an MCP server
   and obtains context for the MCP host to use

1. **MCP Server**: A program that provides context to MCP clients (can run
   locally or remotely)

**Key Relationship**: Each MCP host creates one MCP client for each MCP server.
Each MCP client maintains a dedicated one-to-one connection with its
corresponding MCP server.

### Two-Layer Architecture

#### Data Layer

The data layer implements a **JSON-RPC 2.0** based exchange protocol that
defines the message structure and semantics:

* **Lifecycle Management**: Handles connection initialization, capability
  negotiation, and termination
* **Server Features**: Tools, resources, and prompts
* **Client Features**: Sampling, elicitation, and logging
* **Utility Features**: Notifications and progress tracking

#### Transport Layer

The transport layer manages communication channels and authentication between
clients and servers:

* **Stdio Transport**: Uses standard input/output streams for direct process
  communication between local processes on the same machine (optimal
  performance, no network overhead)

* **Streamable HTTP Transport**: Uses HTTP POST for client-to-server messages
  with optional Server-Sent Events for streaming. Supports standard HTTP
  authentication methods including bearer tokens, API keys, and custom headers.
  MCP recommends using OAuth for tokens.

## Core Concepts

### Primitives

MCP defines three core **primitives** that servers can expose:

1. **Tools**: Executable functions that AI applications can invoke to perform
   actions
   * Examples: File operations, API calls, database queries
   * Discovery method: `tools/list`
   * Execution method: `tools/call`

1. **Resources**: Data sources that provide contextual information to AI
   applications
   * Examples: File contents, database records, API responses
   * Discovery method: `resources/list`
   * Retrieval method: `resources/read`

1. **Prompts**: Reusable templates that help structure interactions with
   language models
   * Examples: System prompts, few-shot examples
   * Discovery method: `prompts/list`
   * Retrieval method: `prompts/get`

### Client-Exposed Primitives

Servers can also access capabilities exposed by MCP clients:

1. **Sampling**: Allows servers to request language model completions from
   the client's AI application (method: `sampling/complete`)

1. **Elicitation**: Allows servers to request additional information from users
   (method: `elicitation/request`)

1. **Logging**: Enables servers to send log messages to clients for debugging
   and monitoring

### Utility Primitives

* **Tasks (Experimental)**: Durable execution wrappers that enable deferred
  result retrieval and status tracking for expensive computations, workflow
  automation, batch processing, and multi-step operations

## Protocol Features

### Lifecycle Management

MCP is a **stateful protocol** that requires lifecycle management to:

1. **Negotiate Protocol Version**: Ensures client and server are using
   compatible protocol versions (e.g., "2025-06-18")

1. **Perform Capability Discovery**: Each party declares what features they
   support, including which primitives they can handle

1. **Exchange Identity Information**: Both parties provide identification and
   versioning information for debugging and compatibility

### Real-Time Notifications

MCP supports **real-time notifications** that enable dynamic updates between
servers and clients:

* Servers can proactively inform clients about changes without being explicitly
  requested
* Example: When a server's available tools change, it sends a
  `tools/list_changed` notification
* Clients react by refreshing their understanding of available capabilities
* **No response required**: Follows JSON-RPC 2.0 notification semantics

### JSON-RPC 2.0 Foundation

MCP uses **JSON-RPC 2.0** as its underlying RPC protocol:

* Client and servers send requests to each other and respond accordingly
* Notifications can be used when no response is required
* Standard message format with unique IDs for request-response correlation

## Workflow Example

### 1. Initialization

Client sends an `initialize` request with:

* `protocolVersion`: Protocol version being used
* `capabilities`: Features the client supports (tools, resources, prompts,
  elicitation)
* `clientInfo`: Client identification and version

Server responds with:

* `protocolVersion`: Negotiated protocol version
* `capabilities`: Features the server supports
* `serverInfo`: Server identification and version

### 2. Tool Discovery

Client sends: `tools/list` request
Server responds with array of available tools, including:

* `name`: Unique identifier for the tool
* `title`: Human-readable display name
* `description`: What the tool does and when to use it
* `inputSchema`: JSON Schema defining expected parameters

### 3. Tool Execution

Client sends: `tools/call` request with:

* `name`: Tool name from discovery response
* `arguments`: Input parameters defined by the tool's schema

Server responds with:

* `content`: Array of content objects (text, images, resources, etc.)

### 4. Real-Time Updates

Server sends: `notifications/tools/list_changed` notification
Client responds by: Requesting updated `tools/list` to refresh capabilities

## MCP Projects & Resources

The Model Context Protocol ecosystem includes:

* **MCP Specification**: Outlines implementation requirements for clients and
  servers
* **MCP SDKs**: Implementations for different programming languages
* **MCP Development Tools**: Including the MCP Inspector for testing and
  debugging
* **MCP Reference Server Implementations**: Reference implementations of MCP
  servers across various domains

## Scope & Limitations

MCP focuses **solely on the protocol for context exchange**. It does **not**
dictatate:

* How AI applications use language models
* How applications manage the provided context
* Specific implementation details (handled by SDKs)

## Key Advantages

1. **Dynamic Tool Discovery**: Tools and resources can be discovered at runtime
   and updated in real-time

1. **Flexible Content System**: Responses can contain multiple content types
   for rich, multi-format interactions

1. **Transport Abstraction**: Same JSON-RPC 2.0 message format works across
   different transport mechanisms

1. **Standardization**: Enables ecosystem of compatible MCP servers that work
   with any MCP host

1. **Real-Time Synchronization**: Notifications ensure clients always have
   accurate information about available capabilities

1. **Authentication Flexibility**: Supports various HTTP authentication methods
   including OAuth

## Getting Started

* **Build Servers**: Create MCP servers to expose your data and tools
* **Build Clients**: Develop applications that connect to MCP servers
* **Explore Servers**: Reference implementations available on GitHub at
  modelcontextprotocol/servers
* **Documentation**: Complete specification and SDKs available at
  [https://modelcontextprotocol.io](https://modelcontextprotocol.io)

## References

* [Model Context Protocol Official Website](https://modelcontextprotocol.io)
* [MCP Specification](https://modelcontextprotocol.io/specification/latest)
* [Architecture Overview](https://modelcontextprotocol.io/docs/learn/architecture)
* [MCP GitHub Repository](https://github.com/modelcontextprotocol)
* [Official MCP Servers](https://github.com/modelcontextprotocol/servers)
