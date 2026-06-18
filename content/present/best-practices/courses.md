# Best Practices - Courses

Based on the curriculum provided in the Anthropic and [DeepLearning.AI](http://deeplearning.ai/) courses, here is a comprehensive list of the best practices taught for working with Claude and Claude Code:

1. Foundational Prompting Best Practices
These principles from Claude 101 and the Claude API course focus on maximizing the model's reasoning capabilities:
    - Give Claude a Role: Assign a specific persona (e.g., "You are a senior site reliability engineer") to set the tone and depth of the response.
    - Use XML Tags: Wrap different parts of your prompt (instructions, examples, data) in XML tags like <docs> or <instructions> to help Claude distinguish between context and commands.
    - Encourage Chain of Thought: Explicitly ask Claude to "think step-by-step" or provide its reasoning before giving a final answer to reduce hallucinations and improve logic.
    - Provide Clear Constraints: Define what the model should not do as clearly as what it should do.
    - Use Context Caching: For the API, cache frequently used context (like large documentation sets) to reduce costs and latency.
2. Agentic Coding Best Practices
Taught specifically in the Claude Code course, these focus on a "Plan-Execute" loop rather than simple generation:
    - The "Plan-First" Workflow: Always ask the agent to create a plan in a `TASK.md` or a markdown block before it writes any code. This allows for human review of the logic before implementation.
    - The `CLAUDE.md` File: Maintain a project-level configuration file that contains specific coding standards, build commands, and architectural notes that the agent should follow every time it enters the environment.
    - Granular File Selection: Use the @ symbol to provide only the relevant files to the agent’s context window rather than the entire repository, which saves tokens and prevents confusion.
    - Parallel Sessions: Use git worktrees to have the agent work on multiple features or bugs in separate terminal sessions without polluting the main branch.
    - Agentic Debugging: Instead of providing the error message and asking for a fix, ask the agent to first reproduce the error with a test case, then fix it.
3. Tool Use & MCP Best Practices
From the Agent Skills and Claude Code courses, focusing on extending Claude's capabilities:
    - Structured Output Requesting: Always request structured data (JSON or XML) when the output is intended for another tool or script to process.
    - Atomic Tool Design: Create "Skills" or tools that do one specific thing well (e.g., "search_database" instead of "manage_data") to make them more reliable and easier for the agent to use.
    - Human-in-the-Loop (HITL): Configure agents to pause and ask for permission before executing "side-effect" actions like deleting files, making API calls that cost money, or deploying code.
    - Model Context Protocol (MCP): Use standardized MCP servers to connect Claude to local databases, Google Drive, or Slack, ensuring a consistent way for the agent to "see" and "interact" with your specific data.
4. Organizational & Workflow Best Practices
From the Claude for Work and Claude Code in Action modules:
    - Golden Datasets: Create a set of "perfect" prompt/response pairs to use as a benchmark when testing new system prompts or model versions.
    - Iterative Refinement: Treat prompts as code, version control them and iterate based on performance data rather than one-off "vibes."
    - Shared Project Knowledge: Use Claude "Projects" to share custom instructions and internal documentation across a team so everyone is working from the same context.
