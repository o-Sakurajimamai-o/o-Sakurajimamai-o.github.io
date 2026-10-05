---
title: "MCP, A2A, ANP"
date: 2026-10-05
description: "Notes on MCP, A2A, and ANP."
draft: false
---

## MCP (Model Context Protocol)

MCP has three layers: the upstream, the MCP protocol, and the downstream.

The downstream is called the MCP Server and is provided by developers or vendors. It implements the specific business logic and registers and exposes three core types of assets through the MCP specification:

- **Tools**: functions that the model can call, such as Git operations, SQL queries, and calls to internal microservices.
- **Resources**: data context that the model can attach in read-only form, such as local files, table schemas, and log streams.
- **Prompts**: standardized prompt templates and workflow presets for interactions.

The upstream is called the MCP Client or host environment: commonly used agent engines such as Codex, Claude Code, and Pi. As clients, they manage lifecycles, protocol translation, and permissions:

- Establish process or network connections with multiple configured MCP Servers.
- Automatically discover the Tools and Resources exposed by each Server and pass them to the LLM when constructing the prompt.
- When the model decides to call a tool, assemble a request according to the standard protocol, dispatch it to the target Server, and return the execution result to the reasoning loop.

Then there is the MCP protocol itself. It is a set of standards—a communication and lifecycle contract based on **JSON-RPC 2.0**—that strictly defines message formats, state transitions, and capability negotiation between the upstream and downstream, much like protocols such as TCP/IP.

Take searching GitHub projects and sorting them by star count as an example of the overall workflow: the host obtains the search tool's name, description, and input Schema through MCP, then provides the tool definition to the model. The model generates the tool name and arguments that conform to the Schema. After checking them, the host sends the request to the MCP Server through `tools/call`. The Server calls the GitHub API and returns the result, which the host then passes to the model. **The tool provider defines the Schema; the model generates the call arguments.**

MCP unifies capability descriptions and invocation interfaces, reducing the need to repeatedly write communication adapters for different services. Each service's business logic, authentication methods, permission requirements, and error handling still need to be implemented or configured separately.

The host can map MCP tools into the tool-calling format supported by the model, so the model usually does not need to handle MCP messages directly. Resources and Prompts use their respective protocol interfaces; their contents should not all be converted into tools or automatically injected into the context by default.

A major advantage of the MCP protocol is its **rich community ecosystem**. Anthropic and community developers have already created many ready-to-use MCP servers covering file systems, databases, API services, and other scenarios. This means you do not need to write tool adapters from scratch and can directly use these verified servers.

1. **Awesome MCP Servers** (https://github.com/punkpeye/awesome-mcp-servers), a community-maintained curated list of MCP servers.
2. **MCP Servers Website** (https://mcpservers.org/), the official MCP server directory website.
3. **Official MCP Servers** (https://github.com/modelcontextprotocol/servers), servers officially maintained by Anthropic.

Custom MCP servers can meet specific needs by encapsulating business logic, accessing private data, optimizing performance for particular use cases, or adding custom functionality. You can also publish your own MCP servers on community platforms.

## A2A (Agent-to-Agent)

The MCP protocol addresses interactions between agents and tools, while the A2A protocol addresses collaboration between agents. In a task that requires multiple agents—such as a researcher, a writer, and an editor—to work together, they need to communicate, delegate tasks, negotiate capabilities, and synchronize their states.

The traditional collaboration architecture, which is still used in most cases today, is a star topology: a lead Agent assigns tasks to multiple sub-agents, then consolidates and reports the results. However, it has several problems:

- **Single point of failure**: if the lead Agent fails, the entire system goes down.
- **Performance bottleneck**: all communication passes through the central node, limiting concurrency.
- **Difficulty scaling**: adding or changing agents requires changes to the central logic.

The A2A protocol adopts a peer-to-peer (P2P) architecture—a mesh topology—that allows agents to communicate directly, fundamentally resolving the problems above. At its core are two abstractions: **Task** and **Artifact**. This is its biggest difference from MCP, as shown in Table 10.7.

![Table 10.7: Comparison of MCP and A2A](figures/10-table-7.png)

To manage the collaboration process, A2A defines a standardized lifecycle for tasks, including states such as creation, negotiation, delegation, execution, completion, and failure.

![A2A task lifecycle](figures/10-7.png)

This mechanism enables agents to negotiate tasks, track progress, and handle exceptions.

The A2A request lifecycle is a sequence that describes the four main steps a request follows: agent discovery, authentication, the Send Message API, and the Send Message Stream API. The diagram below draws on the flowchart from the official website to show the workflow and explain the interactions between the client, the A2A server, and the authentication server.

![A2A request lifecycle and interactions between the client, A2A server, and authentication server](figures/10-8.png)

## ANP (Agent Network Protocol)

When agents are distributed across different platforms or organizations, questions arise about how to describe capabilities, discover services, verify identities, and interact across domains. ANP provides a protocol foundation for these interactions on open networks. Applications can use this information to choose service providers, but the strategy for selecting the most suitable Agent based on real-time load, cost, or service quality still needs to be implemented by the application or platform. ANP's rules for cross-domain communication are not an optimal task-scheduling algorithm.

ANP aims to provide a standardized mechanism to address the service discovery, routing, and network scalability issues described above. To achieve this goal, ANP defines the following core concepts:

![Core concepts of ANP](figures/10-table-8.png)

The official [Getting Started Guide](https://github.com/agent-network-protocol/AgentNetworkProtocol/blob/main/docs/chinese/ANP入门指南.md) illustrates ANP's architectural design.

![ANP architecture and interaction workflow](figures/10-9.png)

In this flowchart, Agent A first queries a public discovery service using semantic or functional descriptions to locate an Agent B that meets its task requirements. The discovery service builds an index by crawling the standard endpoints exposed by agents (`.well-known/agent-descriptions`) in advance, enabling dynamic matching between service requesters and providers.

Agent A then uses its private key to sign a request containing its own DID. Upon receiving the request, Agent B resolves the DID to obtain the public key, verifies integrity and authenticity, and establishes reliable communication.

After authentication succeeds, Agent B responds to the request, and the two parties exchange data or invoke services, such as bookings and queries, using predefined standard interfaces and data formats. Standardized interaction workflows are the foundation of interoperability across platforms and systems.

At the heart of this mechanism is a decentralized foundation of trust built using DIDs, together with dynamic service discovery through a standardized description protocol. This approach enables agents to form secure, efficient collaboration networks on the internet without central coordination.
