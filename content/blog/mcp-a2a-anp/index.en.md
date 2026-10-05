---
title: "MCP, A2A, and ANP: How Agent Protocols Fit Together"
date: 2026-10-05
description: "From tool access to agent collaboration and discovery across organizations: the concepts, interaction flows, and boundaries of MCP, A2A, and ANP."
draft: false
---

While learning about agents, I keep encountering MCP, A2A, and ANP. All three involve connecting systems, but the objects and problems differ. Calling a database tool, delegating research to another agent, and discovering a service operated by another organization require different agreements.

This article develops the MCP, A2A, and ANP chapter in my Agents Notes, following a useful learning sequence: connect capabilities, delegate work, then discover and interact across organizational boundaries. These protocols overlap; they are not three mandatory layers of a network stack.

## What problem does each protocol address?

| Protocol | Main concern | Example |
| --- | --- | --- |
| MCP: Model Context Protocol | A common way for a host to access tools, resources, and prompt templates | Let an agent search GitHub or read a database schema |
| A2A: Agent2Agent Protocol | How independent agents describe capabilities, exchange messages, track tasks, and deliver results | Delegate research to another agent and receive its report |
| ANP: Agent Network Protocol | Service description, discovery, and identity verification on an open network | Discover another organization's service and call its interface with an authenticated request |

The right choice depends on the boundary a system needs to cross. An application can use several of these protocols or implement only one.

## MCP: connecting external capabilities to an agent

### Host, Client, and Server

From a user's perspective, MCP is a shared language between an AI application and external services. Its architecture distinguishes three roles:

- **Host**: the AI application a user interacts with. It manages model integration, context, permissions, and multiple connections.
- **Client**: a protocol component created by the host. Each client maintains an independent session with one server and handles initialization, capability negotiation, and message exchange.
- **Server**: a program or service that provides concrete capabilities backed by files, databases, Git, business APIs, or other systems.

A host and a client are therefore different concepts: one host can manage several clients connected to different servers. A server may run as a local process or as a remote service.

{{< fig src="figures/mcp-flow.en.svg" alt="MCP interaction: a Host contains the model and a Client; the Client connects to a Server, which accesses a business API." caption="MCP roles and the call path. The host manages model interactions, the client handles the protocol session, and the server implements external capabilities." >}}

### Three core capabilities

| Capability | What it provides | Typical use |
| --- | --- | --- |
| **Tools** | Callable operations with names, descriptions, and input schemas | The model selects a tool and generates arguments; the host checks and dispatches the call |
| **Resources** | Context identified by a URI, such as files, database schemas, or logs | The application reads, selects, and manages content to provide to the model |
| **Prompts** | Reusable prompt templates and their parameters | The user selects a template through the host for a particular interaction or workflow |

Tools are typically model controlled, resources application controlled, and prompts user controlled. These are interaction roles; the host still defines the interface, access policy, and context management. Discovering a capability does not automatically place its contents in the model's context.

MCP uses **JSON-RPC 2.0** to define application messages, lifecycle behavior, and capability negotiation. It governs communication between the host and servers. The model usually sees tool definitions and results translated by the host, rather than producing MCP messages itself.

### Following a GitHub search

Suppose the user asks: “Find GitHub projects related to agents and sort them by stars.” The interaction can be broken into six steps:

1. The host creates a client and completes initialization and capability negotiation with a suitable MCP server.
2. The client retrieves available tools through `tools/list`. The host selects relevant definitions to expose to the model.
3. The model generates a tool name and arguments, such as a search term, sort field, and result limit.
4. The host checks the arguments and permissions. The client then sends a `tools/call` request to the server.
5. The server calls the GitHub API, handles authentication, rate limits, and errors, and returns a result.
6. The host passes that result back to the model, which turns it into the requested list.

One distinction matters: **the tool provider defines the input schema; the model generates arguments that conform to it.** The model does not invent the server's interface at call time.

### What remains after standardizing the interface?

MCP reduces repeated work on communication adapters. Each service still needs its own business logic, authentication setup, permissions, and error handling. Wrapping a database in an MCP server does not automatically settle query permissions or execution costs.

Existing implementations can provide a starting point, but their source, maintenance status, and requested permissions still matter:

- [MCP Registry](https://registry.modelcontextprotocol.io/): the official server registry.
- [MCP reference servers](https://github.com/modelcontextprotocol/servers): reference implementations maintained by the MCP steering group. The repository describes them primarily as educational and demonstration implementations.
- [Awesome MCP Servers](https://github.com/punkpeye/awesome-mcp-servers): a community collection of servers.

A custom MCP server can expose private data, internal services, or specialized operations when existing integrations do not fit the application.

## A2A: collaboration between independent agents

### From calling an operation to delegating work

A tool call often names a specific operation, such as searching repositories. A delegated task may be more open ended: ask a research agent to compare several agent frameworks, let it choose its internal steps, and receive progress updates and a final report.

A2A specifies how independent agents exchange capability descriptions, messages, task states, and deliverables without requiring them to expose their internal prompts, memory, or toolchains. An agent can act as a requester in one interaction and a service provider in another.

### Tasks, artifacts, and agent cards

| Concept | Meaning |
| --- | --- |
| **Agent Card** | A description of an agent, including capabilities, interfaces, and security information needed to interact with it |
| **Message** | A unit of communication carrying a user request or an agent reply |
| **Part** | A content unit within a message or artifact, representing text, files, structured data, or other supported content |
| **Task** | A stateful unit of work suitable for ongoing execution or multiple interactions |
| **Artifact** | A deliverable produced by a task, such as a report, file, or structured analysis |

“Research this topic” is a request message. The resulting work can be represented as a task, and its report as an artifact. A simple interaction may return a message directly; not every request needs to create a task.

The existence of tasks is not an absolute dividing line between MCP and A2A. The **2025-11-25** MCP specification also introduced experimental Tasks. A more useful distinction is their main focus: external capability and context integration for MCP, and collaboration between independent agents for A2A.

### Task lifecycle and request flow

For ongoing work, a requester needs to know whether processing continues, whether more information is needed, and whether execution ultimately succeeded. A2A defines states corresponding to submitted, working, input required, authentication required, completed, failed, canceled, and rejected. Input or authentication requirements can interrupt work that may continue; completion, failure, cancellation, and rejection are terminal outcomes.

{{< fig src="figures/a2a-lifecycle.en.svg" alt="Simplified A2A task lifecycle: submitted work progresses, can wait for input or authentication and resume, then completes or ends in failure, cancellation, or rejection." caption="A conceptual view of A2A task states. Some paths are omitted; use the chosen protocol version for exact state values and valid transitions." >}}

A typical interaction involves:

1. **Discovery and description**: obtain the target's Agent Card and inspect its capabilities, interfaces, and supported interaction modes.
2. **Authentication**: obtain the credentials required by the advertised security mechanism and satisfy the service's access policy.
3. **Sending a request**: choose ordinary request/response or streaming interaction according to the service's capabilities and the application's needs. These are alternatives, not two stages that every request must execute in sequence.
4. **Tracking and receiving results**: receive messages, task updates, and artifacts; provide additional input for the relevant task when needed.

A research agent can report that a search is underway, request a narrower date range, and later deliver the report. The caller can follow the work without knowing the agent's internal implementation.

### A2A does not prescribe the entire system topology

Many multi-agent applications use a lead agent to assign work and combine results. Central orchestration can create bottlenecks or a single point of failure, while also making coordination and auditing easier.

A2A gives independent agents a common interaction protocol. **It does not require a peer-to-peer mesh or automatically remove central bottlenecks.** Its official introduction includes an assistant orchestrating other agents. Whether to retain a lead agent, allow direct communication, or distribute coordination remains an architectural decision, along with fault tolerance and scaling.

## ANP: discovery and identity on an open network

### Across platforms, first find the service and its interface

When services belong to different organizations, domains, and platforms, a requester may not already know who provides a capability, where its interface is, or how to access it. The parties may also lack a shared account system.

**ANP stands for Agent Network Protocol.** It provides mechanisms for identity authentication, description, and discovery that help agents obtain the information needed to understand and access another service. Descriptions and discovery can inform selection; choosing the best provider by current load, cost, or quality still requires application policy.

| Concept | Role in an interaction |
| --- | --- |
| **Agent Description** | Describes an agent, available information, and interfaces so a requester can access its capabilities |
| **Discovery endpoint and index** | Publishes description locations that search services can index |
| **DID and DID document** | Identifies an entity and supplies public keys and related information for signature verification |
| **Interface description** | Specifies how to call a service and structure exchanged data |
| **Application selection policy** | Chooses providers using capabilities and available business information; this belongs to application design |

### An example from discovery to execution

Using the `did:wba` approach as an example, Agent A can interact with Agent B as follows:

1. **B publishes a discovery endpoint.** `/.well-known/agent-descriptions` exposes a collection of Agent Description URLs. Individual descriptions point to capability and interface information.
2. **A discovers the target.** A can start from a known domain or query a search service that indexes descriptions. A search index is one possible entry point, not the only one.
3. **A reads the description and prepares a request.** A follows B's interface description. For a request requiring authentication, it signs as specified using a key associated with its DID.
4. **B verifies identity and the request.** B resolves A's DID document, obtains the relevant public key, and checks the signature and associated request validation information.
5. **B checks permissions and executes.** The service separately checks business permissions and any required user authorization, performs the operation, and returns data or results.

{{< fig src="figures/anp-discovery.en.svg" alt="ANP discovery and invocation: B publishes a description, A discovers its interface and sends a signed request, and B resolves A's DID, verifies the request, and checks business permissions." caption="An ANP interaction using did:wba as an example. Discovery, authentication, and business authorization address different questions." >}}

Signature verification helps establish identity and request integrity. It does not independently establish service quality, business permissions, or a user's consent to an operation. Even after identifying a request as coming from A, B must decide whether A may perform that operation.

ANP can reduce dependence on a single agent platform or shared account provider. That does not eliminate infrastructure dependencies: the `did:wba` approach still relies on domains, HTTPS, and the surrounding web trust infrastructure.

## How the three might work together

Consider an application that researches agent frameworks and produces a report:

- A local research agent uses **MCP** to search GitHub, read files, or query an internal database.
- A lead agent uses **A2A** to delegate part of the analysis to a specialist agent, follow its task, and receive a report artifact.
- To access an external service that has not been integrated in advance, the application uses **ANP** descriptions, discovery, and identity mechanisms to find the provider and interact through its advertised interface.

This is an example application architecture. It does not mean the three protocols automatically form a pipeline or are interchangeable. Where two sides use different protocols, the application still needs explicit integration, data mapping, and access control.

The question I find most useful is what the application actually needs to connect. For common tool access, look at MCP. For delegating and tracking work across independent agents, look at A2A. For discovery and identity across domains, look at ANP. Protocols establish interface agreements; scheduling, fault tolerance, business permissions, and result quality remain responsibilities of the surrounding system.

## References

This article expands the “MCP, A2A, ANP” chapter of my Agents Notes, which drew on related Hello-Agents learning material. The diagrams have been redrawn against protocol documentation, and the original image-based concept tables have been rewritten as text. Documentation was checked on **October 5, 2026**. Implementations should pin a specification version; a documented capability does not imply complete support in every SDK.

1. [MCP architecture, 2025-11-25](https://modelcontextprotocol.io/specification/2025-11-25/architecture), and [server primitives](https://modelcontextprotocol.io/specification/2025-11-25/server).
2. [MCP experimental Tasks, 2025-11-25](https://modelcontextprotocol.io/specification/2025-11-25/basic/utilities/tasks).
3. [A2A 1.0.0 specification](https://a2a-protocol.org/v1.0.0/specification).
4. [What is A2A?](https://github.com/a2aproject/A2A/blob/main/docs/topics/what-is-a2a.md) and [Life of a Task](https://github.com/a2aproject/A2A/blob/main/docs/topics/life-of-a-task.md).
5. [ANP introduction](https://github.com/agent-network-protocol/AgentNetworkProtocol/blob/main/docs/chinese/ANP入门指南.md).
6. [ANP DID-based authentication protocol](https://github.com/agent-network-protocol/AgentNetworkProtocol/blob/main/chinese/02-ANP-基于DID的身份认证协议.md).
7. [ANP agent discovery specification](https://github.com/agent-network-protocol/AgentNetworkProtocol/blob/main/chinese/08-ANP-智能体发现协议规范.md).
