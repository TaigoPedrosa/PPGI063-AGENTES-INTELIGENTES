# Guidelines - Synthesis

# Guideline - Intelligent Agents

## Summary

| **Topic** | **Essential Guideline** |
| --- | --- |
| **Intelligent Agents** | Use for complex and iterative tasks |
| **Agent Architecture** | Define model, tools, memory, and permissions |
| **Context Window and Memory** | Control tokens and loaded history |
| **CLAUDE.md** | Create the project constitution |
| **Planning Before Execution** | Plan before executing |
| **MCP** | Connect agents to external tools with control |
| **Subagents** | Use specialized agents with clear scope |
| **Skills, Hooks, and Plugins** | Standardize, validate, and distribute resources |
| **Security** | Audit permissions and critical actions |
| **Tokens and Costs** | Manage AI as an operational cost |

---

## 1. Intelligent Agents

| Section | Content |
| --- | --- |
| **Guideline** | Intelligent agents must be understood as systems capable of planning, executing, using tools, correcting errors, validating outputs, and continuing in cycles until a defined objective is achieved. |
| **Best Practices** | Define clear goals; use agents for complex tasks; limit tool access; validate outputs; monitor cost; require human approval for critical actions. |
| **Common Mistakes** | Treating agents as simple chatbots; giving unrestricted permissions; skipping validation; using agents for trivial tasks; not defining stopping criteria. |
| **Practical Application** | Use in multi-step tasks such as software development, code review, API integration, testing, documentation, data analysis, and automation. |
| **Updated News / Source** | In May 2026, Anthropic announced higher Claude Code usage limits for Pro, Max, Team, and Enterprise plans, reinforcing professional growth of coding agents. Link: [https://www.anthropic.com/news/higher-limits-spacex](https://www.anthropic.com/news/higher-limits-spacex) |

---

## 2. Agent Architecture

| Section | Content |
| --- | --- |
| **Guideline** | An agent should not be treated only as a language model. It should be understood as an architecture composed of model, orchestrator, tools, memory, permissions, and execution control. |
| **Best Practices** | Define model role; orchestration flow; memory strategy; allowed tools; permission levels; logs; validation steps; human approval points. |
| **Common Mistakes** | Ignoring architecture; allowing free tool use; mixing memory with execution; not documenting permissions; deploying agents without monitoring. |
| **Practical Application** | Every agent project must define which tools the agent can use, which files it can access, which commands it can execute, and which actions require human approval. |
| **Updated News / Source** | Anthropic's May 2026 Claude Code expansion shows agentic systems being prepared for larger workloads, longer sessions, and structured execution. Link: [https://www.anthropic.com/news/higher-limits-spacex](https://www.anthropic.com/news/higher-limits-spacex) |

---

## 3. Context Window and Memory

| Section | Content |
| --- | --- |
| **Guideline** | The context window must be treated as a limited resource. Only information relevant to the next decision or execution step should be loaded into the agent's context. |
| **Best Practices** | Use concise context; summarize long histories; retrieve relevant excerpts; remove obsolete data; separate short-term and long-term memory; monitor token usage. |
| **Common Mistakes** | Loading entire files unnecessarily; keeping excessive chat history; mixing outdated information; ignoring token cost; exposing sensitive data without need. |
| **Practical Application** | Avoid excessive history, long logs, unnecessary files, vague prompts, and obsolete information. Summarize or retrieve only the most relevant excerpts. |
| **Updated News / Source** | GitHub announced usage-based billing for Copilot through GitHub AI Credits, calculated from input, output, and cached tokens, making context a cost factor. Link: [https://github.blog/news-insights/company-news/github-copilot-is-moving-to-usage-based-billing/](https://github.blog/news-insights/company-news/github-copilot-is-moving-to-usage-based-billing/) |

---

## 4. `CLAUDE.md`

| Section | Content |
| --- | --- |
| **Guideline** | `CLAUDE.md` should function as the project constitution, bringing together mission, architecture, rules, restrictions, technical standards, commands, and stopping criteria. |
| **Best Practices** | Include project goals; folder structure; commands; coding standards; testing rules; security restrictions; review criteria; clear instructions for the agent. |
| **Common Mistakes** | Using a generic file; writing vague rules; forgetting to update it; adding contradictory instructions; omitting forbidden commands or sensitive files. |
| **Practical Application** | Every Claude Code project should include a guidance file to avoid improvisation, inconsistency, unsafe changes, and decisions misaligned with project goals. |
| **Updated News / Source** | Claude Code documentation reinforces project-level configuration, memory, commands, settings, skills, and structured instructions for agentic workflows. Link: [https://code.claude.com/docs/en/claude-directory](https://code.claude.com/docs/en/claude-directory) |

---

## 5. Planning Before Execution

| Section | Content |
| --- | --- |
| **Guideline** | Complex tasks should follow the flow: plan review → approve → execute → validate. The agent should not act before presenting a clear plan. |
| **Best Practices** | Request a plan; identify risks; list affected files; define tests; separate steps; estimate impact; prepare rollback; require approval before critical execution. |
| **Common Mistakes** | Executing immediately; changing many files at once; skipping tests; ignoring risks; not preparing rollback; treating the plan as optional. |
| **Practical Application** | Before modifying files, running commands, or changing architecture, the agent should identify risks, affected files, validation steps, tests, and rollback options. |
| **Updated News / Source** | Claude Code common workflows emphasize exploration, planning, implementation, testing, and review, reinforcing structured execution before final delivery. Link: [https://code.claude.com/docs/en/common-workflows](https://code.claude.com/docs/en/common-workflows) |

---

## 6. MCP (Model Context Protocol)

| Section | Content |
| --- | --- |
| **Guideline** | MCP should be used to connect AI agents to external tools in a standardized way, such as databases, APIs, GitHub, Figma, dashboards, and file systems. |
| **Best Practices** | Use trusted MCP servers; restrict permissions; separate read and write access; log tool calls; protect credentials; require approval for side effects. |
| **Common Mistakes** | Connecting unknown servers; granting broad access; exposing API keys; allowing write actions without review; not auditing external integrations. |
| **Practical Application** | Use MCP only with trusted servers, limited permissions, secure authentication, logs, and strict control over actions with side effects. |
| **Updated News / Source** | Claude Code documentation presents MCP as a way to connect agents to external tools and data sources, strengthening its role in professional agentic systems. Link: [https://code.claude.com/docs/en/mcp](https://code.claude.com/docs/en/mcp) |

---

## 7. Subagents

| Section | Content |
| --- | --- |
| **Guideline** | Subagents should be used when there are independent or specialized tasks that can be executed in parallel, such as backend, frontend, testing, documentation, or API integration. |
| **Best Practices** | Give each subagent a clear role, scope, input, output, tool limit, validation rule, and escalation path. Use supervision to consolidate results. |
| **Common Mistakes** | Creating too many subagents; giving vague roles; duplicating responsibilities; allowing conflicting outputs; not validating the final integration. |
| **Practical Application** | Each subagent must receive a clear scope, objective, limits, available tools, expected output format, and validation criteria. |
| **Updated News / Source** | Claude Code now documents custom subagents as a native mechanism for delegating specialized tasks, supporting modular multi-agent workflows. Link: [https://code.claude.com/docs/en/sub-agents](https://code.claude.com/docs/en/sub-agents) |

---

## 8. Skills, Hooks, and Plugins

| Section | Content |
| --- | --- |
| **Guideline** | Skills should teach procedures; hooks should enforce mandatory actions; plugins should package and distribute reusable configurations, skills, agents, hooks, and integrations. |
| **Best Practices** | Use skills for repeatable procedures; hooks for tests and security checks; plugins for reusable configurations. Version, audit, and document all resources. |
| **Common Mistakes** | Creating vague skills; ignoring hook failures; installing untrusted plugins; not versioning configurations; relying only on the model to remember rules. |
| **Practical Application** | Use skills for standardization, hooks for security and validation, and plugins to share reusable environments and agent configurations across projects. |
| **Updated News / Source** | Claude Code documentation keeps hooks as a mechanism to execute commands, prompts, validations, or HTTP calls at specific points of the agent lifecycle. Link: [https://code.claude.com/docs/en/hooks/](https://code.claude.com/docs/en/hooks/) <br> [https://code.claude.com/docs/en/skills](https://code.claude.com/docs/en/skills) |

---

## 9. Security

| Section | Content |
| --- | --- |
| **Guideline** | Every use of agents must follow the principle of least privilege: the agent should only access what is necessary to complete the task. |
| **Best Practices** | Limit permissions; protect API keys; use sandboxing; audit tools; block dangerous commands; separate environments; enable logs; require approval for sensitive actions. |
| **Common Mistakes** | Saving secrets in prompts; granting unrestricted file access; allowing production changes; using untrusted tools; not monitoring what the agent executed. |
| **Practical Application** | Audit skills, plugins, MCP servers, permissions, API keys, file access, and shell commands. Block dangerous actions and require human approval for critical operations. |
| **Updated News / Source** | Microsoft introduced the Agent Governance Toolkit in 2026 to support runtime security governance for autonomous AI agents, including policy, identity, and sandboxing. Link: [https://opensource.microsoft.com/blog/2026/04/02/introducing-the-agent-governance-toolkit-open-source-runtime-security-for-ai-agents/](https://opensource.microsoft.com/blog/2026/04/02/introducing-the-agent-governance-toolkit-open-source-runtime-security-for-ai-agents/) |

---

## 10. Tokens and Costs

| Section | Content |
| --- | --- |
| **Guideline** | Tokens must be treated as computational and financial resources. AI usage must consider input, output, cache, model selection, execution time, and repeated attempts. |
| **Best Practices** | Use smaller models for simple tasks; optimize context; apply caching; set budgets; monitor usage; avoid long unnecessary prompts; measure cost per task. |
| **Common Mistakes** | Using expensive models for everything; ignoring token consumption; loading full documents; allowing unlimited loops; not tracking cost by user or project. |
| **Practical Application** | Use smaller models for simple tasks, advanced models for critical decisions, reduce unnecessary context, avoid overly long prompts, and monitor cost per task. |
| **Updated News / Source** | OpenAI's Codex pricing page shows that coding agents are organized through specific usage plans, reinforcing the need for budgets, limits, and cost awareness. Link: [https://developers.openai.com/codex/pricing](https://developers.openai.com/codex/pricing) |

|  |  |
| --- | --- |
|  |  |
|  |  |
|  |  |
|  |  |
|  |  |
|  |  |
|  |  |
