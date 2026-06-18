# Claude Code Commands, Claude Cowork & Claude Design

## 1. Claude Code slash commands

Each command links to the official documentation, with an explanation and a practical example. In the examples, `>` represents the Claude Code prompt. Full reference at [code.claude.com/docs/en/commands](https://code.claude.com/docs/en/commands).

### [`/usage`](https://code.claude.com/docs/en/costs#using-the-%2Fusage-command)

Shows the current session cost, your plan's usage limits, and activity stats. On Pro, Max, Team, and Enterprise plans, it includes a breakdown of how much each skill, subagent, plugin, and MCP server consumed. It came from merging `/cost` and `/stats` (which remain as aliases).

```
> /usage
```

Opens the panel; run it before a long task to see how much of your limit is left.

### [`/skills`](https://code.claude.com/docs/en/skills)

Lists all skills available in the session. Helps you discover what's loaded and control what bloats the context.

```
> /skills      (then press t)
```

Lists skills and sorts by token count; `Space` hides a skill from the menu and `Enter` saves.

### [`/workflows`](https://code.claude.com/docs/en/workflows)

Opens the progress view for *dynamic workflows*, runs that fan work out across many subagents in the background.

```
> /workflows
```

Watch, pause, resume, or save running and completed runs.

### [`/code-review`](https://code.claude.com/docs/en/code-review)

Reviews the current diff for correctness bugs and cleanup opportunities (reuse, simplification, efficiency). Accepts effort levels (`low` → `max`) and can apply the fixes or post them as PR comments.

```
> /code-review high --fix
```

Reviews at high effort and applies the fixes to your working tree; use `ultra` for a cloud review or `--comment` to comment on the PR.

### [`/simplify`](https://code.claude.com/docs/en/code-review)

A cleanup-only review: four agents run in parallel covering reuse of existing helpers, simplification, efficiency, and the right level of abstraction, and apply the fixes. From v2.1.154 it does not look for correctness bugs (use `/code-review` for that).

```
> /simplify src/auth/
```

Cleans up the code in the given directory, applying the improvements automatically.

### [`/ultrareview`](https://code.claude.com/docs/en/ultrareview)

A deep, multi-agent code review running in a cloud sandbox. The preferred invocation today is `/code-review ultra`; `/ultrareview` remains as an alias. Includes 3 free runs on Pro and Max.

```
> /ultrareview 1234
```

Reviews PR #1234 in the cloud with several agents in parallel.

### [`/goal`](https://code.claude.com/docs/en/goal)

Sets a persistent goal: Claude keeps working across turns until the condition is met, instead of stopping after each response.

```
> /goal all tests passing and lint clean
```

Claude keeps iterating until it's done; `/goal clear` cancels the active goal.

### [`/fewer-permission-prompts`](https://code.claude.com/docs/en/commands)

Reduces repeated permission prompts. Scans your transcripts, identifies frequently used read-only Bash and MCP calls, and adds a prioritized allowlist to `.claude/settings.json`. *(Previously named `/less-permission-prompts`.)*

```
> /fewer-permission-prompts
```

Generates the suggested allowlist for you to review and approve.

### [`/reload-skills`](https://code.claude.com/docs/en/skills)

Re-scans the skill and command directories so items created or changed on disk during the session become available without restarting.

```
> /reload-skills
```

After creating `.claude/skills/deploy/SKILL.md`, this command makes it usable right away.

### [`/chrome`](https://code.claude.com/docs/en/chrome)

Configures Claude in Chrome, the browser agent.

```
> /chrome
```

Chooses which connected browser the agent will control.

### [`/scroll-speed`](https://code.claude.com/docs/en/fullscreen#mouse-wheel-scrolling)

Adjusts the mouse wheel scroll speed. Available in fullscreen rendering only.

```
> /scroll-speed
```

Opens an interactive ruler to preview and apply the speed.

### [`/recap`](https://code.claude.com/docs/en/interactive-mode#session-recap)

Generates, on demand, a one-line summary of the current session. (There's also the automatic recap that appears when you return after a while.)

```
> /recap
```

Handy when resuming a session to remember where you left off.

### [`/powerup`](https://code.claude.com/docs/en/commands)

Introduces Claude Code features through short interactive lessons with animated demos.

```
> /powerup
```

Opens the mini-tutorials to explore features you might not know about.

### [`/team-onboarding`](https://code.claude.com/docs/en/commands)

Generates a team onboarding guide from your usage history. It analyzes `CLAUDE.md`, skills, subagents, hooks, and your commands/MCP servers from the past 30 days.

```
> /team-onboarding
```

Produces a markdown guide a teammate can paste as a first message to get set up quickly.

### [`/usage-credits`](https://code.claude.com/docs/en/commands)

Configures usage credits to keep working after hitting your plan's limit. *(Previously named `/extra-usage`.)*

```
> /usage-credits
```

Opens the extra-credits configuration.

### [`/btw`](https://code.claude.com/docs/en/interactive-mode#side-questions-with-%2Fbtw)

Asks a quick side question without adding anything to the main conversation's context, great for a quick question without bloating the history.

```
> /btw what's the difference between git rebase and merge?
```

Answers on the side, without polluting the task in progress.

### [`/schedule`](https://code.claude.com/docs/en/routines)

Creates, updates, lists, or runs *routines*, tasks that execute on Anthropic-managed cloud infrastructure. Claude walks you through the setup conversationally. *(Alias: `/routines`.)*

```
> /schedule run the tests and open a PR every Monday at 9am
```

Claude sets up the routine and it then runs on its own at the scheduled time.

### [`/background`](https://code.claude.com/docs/en/agent-view)

Detaches the current session to run as a *background agent*, freeing the terminal. You can send one last instruction before detaching. *(Alias: `/bg`.)*

```
> /background keep refactoring while I'm away
```

The session keeps running in the background; monitor it with `claude agents`.

### [`/batch`](https://code.claude.com/docs/en/agents)

Orchestrates large-scale code changes in parallel. It researches the repo, decomposes the work into 5–30 independent units, and, once approved, spawns one subagent per unit in an isolated git worktree, opening one PR per unit.

```
> /batch migrate src/ from Solid to React
```

Each unit runs in isolation, runs tests, and opens its own PR.

### [`/deep-research`](https://code.claude.com/docs/en/workflows)

A workflow that fans out web searches in parallel on a question, fetches and cross-checks sources, and synthesizes a cited report.

```
> /deep-research compare task-queue libraries for Node
```

Delivers a cited report instead of a single answer.

### [`/run`](https://code.claude.com/docs/en/skills)

Launches and drives your project's app so you can see the change working in the real app, not just in tests. *(Requires v2.1.145+.)*

```
> /run
```

Starts the app and navigates to the changed screen to confirm it visually.

### [`/verify`](https://code.claude.com/docs/en/skills)

Confirms a change does what it should by building the project, running the app, and observing the result, instead of relying only on tests or type checks. *(Requires v2.1.145+.)*

```
> /verify
```

Validates the change by running the app and checking the real behavior.

### [`/teleport`](https://code.claude.com/docs/en/claude-code-on-the-web)

Pulls a Claude Code on the web session into the current terminal: opens a picker and fetches the branch and conversation. *(Alias: `/tp`; requires a [claude.ai](http://claude.ai/) subscription.)*

```
> /teleport
```

Brings the web session over to continue locally.

### [`/ultraplan`](https://code.claude.com/docs/en/ultraplan)

Drafts a plan in a cloud *ultraplan* session; you review it in the browser, then execute remotely or send the plan back to your terminal.

```
> /ultraplan add OAuth authentication to the backend
```

Generates the plan in the cloud for review before executing.

### [`/autofix-pr`](https://code.claude.com/docs/en/claude-code-on-the-web)

Opens a Claude Code on the web session that watches the current branch's PR and pushes fixes when CI fails or reviewers leave comments. By default it tries to fix everything; pass a prompt to narrow it down. *(Requires the `gh` CLI.)*

```
> /autofix-pr only fix lint and type errors
```

Limits what the remote session will fix on the PR.

### [`/insights`](https://code.claude.com/docs/en/commands)

Generates a report analyzing your Claude Code sessions: project areas you touch most, interaction patterns, and friction points.

```
> /insights
```

Produces the usage report to understand your own habits.

### [`/claude-api`](https://code.claude.com/docs/en/skills)

Loads Claude API reference material for your project's language (Python, TypeScript, Java, Go, Ruby, C#, PHP, or cURL), covering tool use, streaming, batches, and common pitfalls. It also activates on its own when your code imports `anthropic`/`@anthropic-ai/sdk`.

```
> /claude-api migrate
```

Upgrades existing code to a newer model (model IDs, thinking config, etc.).

---

## 2. Claude Cowork

**What it is.** An agentic knowledge-work desktop app (macOS and Windows, also reachable remotely from the mobile app), aimed at people who aren't developers. Think of it as the equivalent of Claude Code, but for "office work" rather than programming: instead of just answering, Claude executes multi-step tasks on its own using tools.

**What it's for.** You delegate a goal and let the agent carry it through to a finished result. It works with files, spreadsheets, documents, and slides, and connects to enterprise apps, chaining steps and passing context between them. It's a good fit for repetitive or time-consuming workflows in operations, marketing, finance, and similar areas, where you'd rather describe the outcome than do every click yourself.

**How it connects to other tools.** Cowork can drive Anthropic's other agents as tools: Claude in Excel (spreadsheet agent), Claude in PowerPoint (slides agent), Claude in Chrome (browser agent), and Claude Design (canvas). It also uses plugins and enterprise connectors, so a single task can pull data, transform it, and produce a deliverable across several apps.

**Practical use cases.**

- "Read this sales spreadsheet, build a summary, and turn it into a deck", Cowork analyzes the data in Excel and hands the context to PowerPoint to generate the slides.
- Monthly reporting: pull numbers from a connected source, update a tracker, and draft the narrative in a Word doc.
- Marketing ops: take a campaign brief, gather references from the web (via Chrome), and assemble a one-pager.
- Live artifacts: produce a dashboard or doc whose code re-runs when reopened, so the output stays current.

---

## 3. Claude Design

**What it is.** A design tool with a canvas where Claude creates and iterates on visual artifacts from a chat conversation. It launched as a research preview, the first public product from Anthropic Labs, and runs on Claude Opus 4.7.

**What it's for.** You describe what you want, a UI prototype, slides, a one-pager, a mockup, and Claude generates a first version. You then refine it through direct edits on the canvas, inline comments, and sliders, instead of starting over for each adjustment. The aim is to go from idea to a polished visual quickly, keeping a human in the loop on the look and feel.

**How it connects to other tools.** A standout is that it's *code-aware*: it can read the React components in your repository so the design comes out aligned with what already exists. It exports to formats like PDF, PPTX, and HTML, and integrates with Canva. Claude Cowork can also use Claude Design as one of its tools.

**Practical use cases.**

- Spin up a clickable prototype of a new feature to share with stakeholders before any engineering work.
- Generate an on-brand slide deck or one-pager from a rough outline, then tweak layout and copy inline.
- Produce UI mockups that match your existing component library, then export to hand off to developers.
- Iterate on a landing-page concept conversationally, adjusting spacing and hierarchy with sliders.

**Availability.** Included for Pro, Max, Team, and Enterprise subscribers at [claude.ai/design](http://claude.ai/design) (uses the weekly token limit).

---

## In short

- **Cowork** is for *executing knowledge work end to end*, delegating multi-step office tasks to an autonomous agent.
- **Design** is for *creating and refining visual pieces* conversationally, from prototypes to slides, with a human steering the look.

---
