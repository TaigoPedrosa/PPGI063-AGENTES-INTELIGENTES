# Best Practices - Subagents

# Guideline: MCP and Sub-agents in Intelligent Agents

## 1) Scope and objective

This guideline consolidates practical guidance on **Model Context Protocol (MCP)** and **sub-agents** based on:

1. the presentation `PPGI_Agentes_Inteligentes___Claude_Code.pdf`;
2. the official MCP documentation;
3. the official Claude Code documentation;
4. technical specifications cited by the documentation itself.

Objective: to provide reference material for the design, operation, and governance of agentic systems that integrate external tools and run tasks in parallel and in isolation.

---

## 2) MCP (Model Context Protocol)

## 2.1 Definition

MCP is an **open standard** for connecting AI applications to external data sources and tools, reducing point-to-point integrations between each agent and each service.

- In the presentation: MCP appears as a solution to the **N x M** integration problem (slide 19) and is described as an open standard with communication via **JSON-RPC 2.0** (slide 20).
- In the official documentation: MCP is presented as an open standard for integrating LLM applications with data, tools, and workflows (`What is MCP?`).

## 2.2 Conceptual architecture (Host, Client, Server)

Role model:

- **Host**: the AI application that orchestrates interaction with the user;
- **Client**: the component within the host that maintains a connection to an MCP server;
- **Server**: a local or remote process that exposes capabilities (tools, resources, prompts).

This model is in the presentation (slide 20) and in the MCP specification (overview and architecture).

## 2.3 Core primitives

MCP organizes capabilities into three groups (slide 21; MCP specification):

- **Resources**: read-only data (files, records, API responses);
- **Prompts**: reusable templates for interactions and workflows;
- **Tools**: executable functions with potential side effects (calling an API, writing to a database, running commands).

## 2.4 Transport and base protocol

- The base protocol uses **JSON-RPC 2.0** (presentation slide 20; MCP specification).
- Common transports cited: **STDIO** (local process) and **HTTP/SSE** (remote), with the note that SSE appears historically but may be deprecated in some specific clients (see the Claude Code docs for current behavior).

## 2.5 Expected practical benefits

1. **Interoperability**: build once and integrate with multiple compatible hosts.
2. **Reduced coupling**: tool servers decoupled from the AI application.
3. **Integration scalability**: avoids the proliferation of proprietary connectors.
4. **Capability standardization**: resources, prompts, and tools with common semantics.

## 2.6 Security and governance (guidelines)

Based on the presentation (slide 22) and the MCP specification (Security and Trust & Safety section):

1. Require **explicit consent** from the user for data access and action execution.
2. Treat tool descriptions and external content as **untrusted by default**.
3. Apply authorization controls and the principle of least privilege per server/tool.
4. Log audit trails of tool calls and critical results.
5. In OAuth integrations, use audience/resource restriction mechanisms where applicable (conceptual basis aligned with RFC 8707).

---

## 3) Sub-agents

## 3.1 Operational definition

A sub-agent is a separate agent instance, created by the orchestrator to run a specific subtask in an **isolated context**.

- Presentation (slide 25): highlights parallelism, context isolation, specialization, and fault tolerance.
- Claude Code docs: sub-agents have their own context window, can have their own prompt/tool scope, and return a summary to the main flow.

## 3.2 Orchestration patterns

### A) Orchestrator-Subagent (parallel)

Typical flow (slide 27):

1. objective received by the orchestrator;
2. decomposition into independent subtasks;
3. parallel execution;
4. collection of results;
5. synthesis/iteration.

Use this when subtasks are independent and benefit from concurrency.

### B) Sequential pipeline

Typical flow (slide 27):

1. agent A produces an artifact;
2. agent B validates it;
3. agent C transforms/integrates it.

Use this when there is a strong dependency between stages and a need for quality gates.

## 3.3 Isolation and result integration

Guidelines:

1. Define a clear input/output contract for each sub-agent.
2. Avoid implicit state sharing; pass data explicitly.
3. Consolidate the return value into a standardized format (findings, evidence, next steps).
4. When available, use worktree isolation to sandbox changes.

## 3.4 Limitations and risks

Per the presentation (slide 28) and the official docs:

- **Cost/token** usage grows with the number of sub-agents;
- **No automatic shared state**;
- **Local failures** require recovery by the orchestrator;
- **Risk of prompt injection propagation** across artifacts/results;
- **Operational latency** can increase in long chains.

Recommended mitigations:

1. validate sub-agent output before promoting it to the main context;
2. limit tool scope by role;
3. standardize the retry, timeout, and fallback policy;
4. use sub-agents only when there is a real gain in isolation or parallelism.

---

## 4) Adoption guide (checklist)

## 4.1 For MCP

- [ ]  Map priority external tools/data.
- [ ]  Define MCP servers per domain (e.g., design, SCM, database, observability).
- [ ]  Classify each tool by risk and permission level.
- [ ]  Implement a consent policy, logging, and security review.
- [ ]  Measure context impact and adjust the tool-loading strategy.

## 4.2 For sub-agents

- [ ]  Define a catalog of roles (e.g., research, implementation, testing, review).
- [ ]  Establish a prompt template and output format per role.
- [ ]  Choose an execution pattern (parallel vs pipeline) per task type.
- [ ]  Adopt criteria for "when not to use a sub-agent".
- [ ]  Instrument metrics for cost, latency, and rework rate.

---

## 5) When to use MCP and sub-agents together

Use the MCP + sub-agents combination when:

1. there are multiple external integrations to query in parallel;
2. each subtask requires a distinct set of tools/permissions;
3. the volume of output could saturate the main thread's context;
4. it is important to separate data research/extraction from the final synthesis stage.

Design example:

- Sub-agent A: gathers requirements from an issue tracker (MCP).
- Sub-agent B: queries operational data/errors (MCP).
- Sub-agent C: assesses the impact on code and tests.
- Orchestrator: consolidates, prioritizes, and proposes an executable plan.

---

## 6) References

### 6.1 Primary source of the presentation

1. Wagner, D. L. P.; Taígo; João Victor. **Intelligent Agents: A Study with Claude Code (Netflix Clone Use Case)**. PPGI, 2026. File: `PPGI_Agentes_Inteligentes___Claude_Code.pdf`.
    - Topics used: MCP (slides 18-23) and Sub-agents (slides 24-28).

### 6.2 Official documentation

1. Model Context Protocol. **What is MCP?** Available at: `https://modelcontextprotocol.io/introduction`.
2. Model Context Protocol. **Specification (2025-03-26)**. Available at: `https://modelcontextprotocol.io/specification/2025-03-26`.
3. Anthropic (Claude Code Docs). **Connect Claude Code to tools via MCP**. Available at: `https://docs.anthropic.com/en/docs/claude-code/mcp`.
4. Anthropic (Claude Code Docs). **Create custom subagents**. Available at: `https://docs.anthropic.com/en/docs/claude-code/sub-agents`.

### 6.3 Related technical specifications

1. JSON-RPC Working Group. **JSON-RPC 2.0 Specification**. Available at: `https://www.jsonrpc.org/specification`.
2. Campbell, B.; Bradley, J.; Tschofenig, H. **RFC 8707 - Resource Indicators for OAuth 2.0**. IETF, 2020. Available at: `https://www.rfc-editor.org/rfc/rfc8707`.

---

## 7) Methodological note

This document avoids untraceable claims. When a detail depends on a specific implementation (for example, transport behavior, tool loading, or background execution rules), the priority reference is the official product documentation (Claude Code docs) and, for protocol semantics, the official MCP specification.
