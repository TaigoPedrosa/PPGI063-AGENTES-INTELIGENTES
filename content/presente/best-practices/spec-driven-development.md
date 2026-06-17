# Best Practices - Spec-Driven Development

## 1) Scope and objective

Spec-Driven Development (SDD) means writing down what you want before the agent builds it, then treating that description as the source of truth instead of throwing it away once the code exists. This page is about doing that well with the two tools that have done the most to popularize it, **GitHub Spec Kit** and **Superpowers**. They come at it from different angles, one an agent-agnostic toolkit and one a skills library that bakes in a fuller engineering method, which is what makes them worth understanding as a pair.

The aim is practical rather than exhaustive: when the extra structure actually earns its keep, how each tool behaves once you get past the pitch and start using it, and how to combine them without drowning in specs nobody reads. Everything here comes from the projects' own repositories, CLIs and documentation, the maintainers' writing, and reports from teams using both through 2025 and 2026.

---

## 2) What Spec-Driven Development is

## 2.1 The core idea

In a normal "vibe coding" loop you describe a feature in chat, the agent writes code, and the prompt that produced it is gone the moment you hit enter. SDD flips the order. You write a specification first, treat it as the source of truth, and have the agent generate the plan, the tasks and the code from that spec. The artifact survives the conversation, so the next agent (or the next teammate) starts from the same understanding instead of guessing.

> We tend to treat coding agents like search engines when we should treat them more like literal minded pair programmers. A literal minded collaborator does much better work when you hand it a clear contract.

- Den Delimarsky, leads Spec Kit at GitHub.
> 

## 2.2 Why it showed up now

Two things made SDD practical in late 2025. Agents got good enough to follow a multi step plan without losing the thread, and the cost of writing the spec dropped because the agent helps you write it. The pitch is that a few minutes spent pinning down intent saves you from the classic failure mode where the agent confidently builds the wrong thing very fast.

## 2.3 The general loop

Most SDD tooling, Spec Kit included, walks through some version of this loop:

1. **Principles** - capture the non negotiables for the project (stack, conventions, quality bar).
2. **Specify** - describe what you want and why, in terms of behaviour and user stories, not implementation.
3. **Clarify** - resolve the gaps and ambiguities before any planning happens.
4. **Plan** - choose the technical approach and the stack.
5. **Tasks** - break the plan into small, checkable units of work.
6. **Implement** - let the agent execute the tasks against the plan.
7. **Verify** - check the result against the spec, not against your memory of the chat.

## 2.4 What it is not

SDD is not a return to a 200 page requirements document signed off before anyone writes code. The specs are meant to be living artifacts that you revise as you learn. It is also not a fit for everything. For a one line bug fix the ceremony costs more than it returns.

---

## 3) Spec Kit (GitHub)

## 3.1 What it is

Spec Kit is an open source toolkit from GitHub (MIT licensed) that brings a structured SDD workflow to any coding agent. It ships a CLI called `specify`, a set of prompt templates, and a folder layout that keeps the spec, plan and tasks together.

The important design choice is that Spec Kit is agent agnostic. It installs slash commands and templates into your repo, and the commands work across more than thirty CLI and IDE based assistants, including GitHub Copilot, Claude Code, Gemini CLI, Cursor and Codex CLI.

## 3.2 How to install and start

The CLI installs through `uv` (Python 3.11 or newer and Git are prerequisites):

```bash
uv tool install specify-cli --from git+https://github.com/github/spec-kit.git
```

Then scaffold a project, pointing it at your agent of choice:

```bash
specify init my-project --integration claude
cd my-project
```

Useful flags: `--here` initializes in the current directory, `--force` skips the prompt on a non empty folder, and `--ignore-agent-tools` pulls the templates without checking that the agent is installed. Run `specify integration list` to see every supported assistant in your installed version.

## 3.3 The workflow commands

Once initialized, you drive everything from slash commands inside your agent. Note the `speckit.` namespace, which is the current naming:

| Command | What it does |
| --- | --- |
| `/speckit.constitution` | Create or update the project's governing principles and quality rules. |
| `/speckit.specify` | Describe what to build, as requirements and user stories. |
| `/speckit.clarify` | Surface and resolve underspecified areas. Run it before planning. |
| `/speckit.plan` | Produce the technical plan and pick the stack. |
| `/speckit.tasks` | Generate an actionable, ordered task list. |
| `/speckit.analyze` | Check the spec, plan and tasks for consistency and coverage gaps. |
| `/speckit.implement` | Execute the tasks and build the feature. |
| `/speckit.checklist` | Generate a custom quality checklist for the feature. |
| `/speckit.taskstoissues` | Turn the task list into GitHub issues. |

## 3.4 What it produces

Spec Kit keeps its scaffolding in `.specify/` and the actual work under `specs/`:

```
.specify/
  memory/constitution.md      # project principles, the contract every step honours
  scripts/                    # helper scripts the commands call
  templates/                  # spec, plan and tasks templates
specs/
  <feature-name>/
    spec.md                   # the what and why
    plan.md                   # the how, with the chosen stack
    tasks.md                  # the ordered, checkable breakdown
    research.md               # technology investigation notes
    data-model.md             # schema and entities
    quickstart.md             # how to run and validate
    contracts/                # API contracts
```

Each feature gets its own branch and its own folder, so the spec travels with the code that implements it.

## 3.5 Advantages

1. **Agent agnostic.** The same workflow moves with you across Copilot, Claude Code, Gemini and thirty other assistants, so you are not tied to one vendor.
2. **The intent outlives the chat.** A spec in the repo is reviewable, diffable and reusable, so nobody has to rebuild your reasoning from the code later.
3. **Clarification is forced, not optional.** The clarify and analyze steps surface contradictions while they are still cheap to fix.
4. **Fits how teams already work.** Specs are plain markdown under version control, so they go through the same pull request review as code.
5. **Earns its keep on real design work.** For greenfield builds and large features, the up front structure pays for itself.

## 3.6 Disadvantages and limitations

1. **It can manufacture the illusion of work.** The agent will happily generate a wall of specs, many of them not the ones you wanted, and you spend the next hour deleting half and rewriting the rest.
2. **A green checkmark is not a working feature.** Teams have seen `/speckit.implement` declare a spec fully done while the code is missing most of the functionality and ships zero tests. You still have to verify it yourself.
3. **The path from spec to debugging is fuzzy.** When something breaks, it is rarely clear whether to patch the code or rewrite the spec, and the workflow does not answer that.
4. **Waterfall can sneak back in.** Gojko Adzic and others warn that heavy up front specification quietly reintroduces the rigidity agile spent two decades escaping.
5. **Specs sprawl as the source of truth.** Once a system spans a dozen feature specs, answering "what does this actually do" means reading and reconciling all of them against the code.
6. **It inherits LLM non determinism.** A flow that works beautifully one day can misfire the next.
7. **Not worth it for small changes.** For bug fixes and minor tweaks, the overhead outweighs the payoff.

## 3.7 Practical notes

Treat the generated specs as a draft to prune, not as output to accept. Keep the constitution short and real, since it is the part the agent leans on most. And do not skip your own verification just because `/speckit.implement` reported success.

---

## 4) Superpowers (obra)

## 4.1 What it is

Superpowers comes at SDD from a different direction than Spec Kit. Rather than a CLI and a fixed repo layout, it is a Claude Code plugin that installs a library of composable **skills**, each one a small markdown file that encodes a proven workflow. The author, Jesse Vincent (obra), describes it as a complete software development methodology for coding agents, built on top of those skills.

It was first published in October 2025, accepted into Anthropic's official plugin marketplace in January 2026, and now sits at its version 5 line. It is MIT licensed and maintained by Vincent with the Prime Radiant team.

It is spec-driven in its own right. The `brainstorming` skill interrogates the idea and writes the agreed design to a spec file, and `writing-plans` turns that into a detailed implementation plan, both committed before any code is written. What sets it apart from Spec Kit is everything around those artifacts. It also enforces how the code gets built: tests before features, a structured review between tasks rather than one review at the end, and evidence of verification before anything is called done.

## 4.2 How to install

From the Anthropic official marketplace:

```
/plugin install superpowers@claude-plugins-official
```

Or from the author's own marketplace, which tends to get updates first:

```
/plugin marketplace add obra/superpowers-marketplace
/plugin install superpowers@superpowers-marketplace
```

Once a plugin system like Claude Code or Cursor has it installed, new versions auto update.

## 4.3 How skills work

A skill is a `SKILL.md` file with a short frontmatter block (a `name` and a `description` that says when to use it) followed by the actual guidance. Skills load on demand through progressive disclosure, so they cost context only when they are relevant. An entry skill, `using-superpowers`, loads at the start of a session and teaches the agent how to find and trigger the rest.

Skills come in two flavours. Rigid skills, such as test driven development, are meant to be followed exactly. Flexible skills carry principles you adapt to the situation. The skill itself tells you which kind it is.

## 4.4 The workflow it encodes

The skills chain into an end to end loop. The core ones:

- **brainstorming** - interrogates a rough idea, explores alternatives, then writes the agreed design to a spec file and commits it before any code exists.
- **writing-plans** - turns that spec into a written implementation plan, broken into two to five minute tasks with exact file paths and verification steps.
- **executing-plans** - runs the plan in batches with human checkpoints between them.
- **subagent-driven-development** - dispatches a fresh subagent per task, then runs a two stage review: first a spec review that checks the work matches the plan, and only after that signs off, a code review for quality.
- **test-driven-development** - enforces the red, green, refactor cycle: write a failing test, watch it fail, write the minimum code to pass.
- **systematic-debugging** - a four phase root cause process instead of guess and check.
- **verification-before-completion** - demands evidence that a fix works before anyone claims it is done.
- **requesting-code-review** and **receiving-code-review** - checklists for asking for review and for responding to feedback without rubber stamping it.

For authoring your own, the `writing-skills` skill covers how to structure and test a new one, treating skill writing as test driven development applied to documentation. (The separate `skill-creator` plugin from Anthropic offers complementary tooling and evals for the same underlying skills mechanism.)

## 4.5 Advantages

1. **Encodes good habits as defaults.** The discipline most teams know they should follow, tests first, plan first, verify before claiming done, becomes the path of least resistance.
2. **The two stage review is genuinely useful.** Separating "does this do what we agreed" from "is this good code" catches a class of problems a single review tends to miss.
3. **Composable and inspectable.** Skills are small markdown files you can read, fork and adapt. Nothing is hidden.
4. **Low setup cost.** It is one plugin install, and it auto updates from then on.
5. **Cross platform.** It runs on Claude Code, Codex CLI and App, Gemini CLI, Cursor, OpenCode, Copilot CLI and others, not just Claude Code where it started.
6. **Actively developed.** Version 5 reworked how git worktrees are used now that the major harnesses support them first class.

## 4.6 Disadvantages and limitations

1. **Context overhead.** Every skill that loads spends tokens. On a large task the methodology itself is a meaningful share of the budget.
2. **It assumes you want the process.** If you just want a quick answer, the insistence on brainstorming and planning first can feel like friction. It is built for people who want the discipline.
3. **Triggering is probabilistic.** Skills fire based on the model deciding they are relevant, so the right one does not always activate, and the wrong one sometimes does.
4. **Most mature on Claude Code.** Support elsewhere is real but the experience is best in the harness it was born in.
5. **Opinionated by design.** It bakes in one philosophy of how to build software. If your team works differently, you will be fighting it rather than using it.

## 4.7 Practical notes

Superpowers shines when you actually let it run the full loop. The common mistake is to invoke it and then skip straight to implementation, which throws away most of the value. Let the brainstorming and planning skills do their job, and lean on `verification-before-completion` before you call anything finished.

---

## 5) Spec Kit and Superpowers side by side

Both are spec-driven, and both write a spec and an implementation plan before any code, so they overlap more than their packaging suggests. Where they differ:

|  | Spec Kit | Superpowers |
| --- | --- | --- |
| Form | CLI, templates and a fixed repo layout | Claude Code plugin, library of skills |
| What it produces | `spec.md`, `plan.md`, `tasks.md` and contracts under `specs/` | A committed design spec and implementation plan, then tests and code |
| Emphasis | The artifacts and their standard structure | The method around the artifacts: TDD, review, verification |
| Agent coupling | Agnostic, thirty plus assistants | Cross platform, strongest on Claude Code |
| Best for | Greenfield and large features with real design | Work where you want the whole engineering loop enforced |
| Main weakness | Can generate documents nobody needs | Context cost and built in opinion |
| Maintainer | GitHub | Jesse Vincent (obra) and Prime Radiant |

Because they overlap, you would not usually run both end to end. If you do combine them, the natural split is Spec Kit for the standardized, agent-agnostic spec and task structure, and Superpowers' test, execution and review skills to build against it. Otherwise pick by emphasis: Spec Kit when the artifact structure is the point, Superpowers when the enforced engineering loop is.

---

## 6) Clean-room engineering and hidden-test evaluation

## 6.1 The idea

Clean-room evaluation separates the thing that writes the code from the thing that judges it. The implementation agent works from the spec and never sees the tests that grade it. Those hidden tests are the oracle. Because the agent cannot read the test cases, it has to satisfy the spec rather than the cases, which is exactly what you want.

The failure this guards against is familiar to anyone who has watched an agent "pass" a task. When the agent can see the tests, it optimizes for them. It hardcodes the expected output, special cases the sample inputs, or writes something that clears the three given examples and nothing else. That is Goodhart's law in miniature: a measure that becomes a target stops being a good measure. Hiding the tests keeps them an honest one.

The technique borrows from two older ideas. One is clean-room reverse engineering from software law. The other is the held-out test set from machine learning. Both rest on the same rule: the producer must not have access to the judge.

## 6.2 The clean-room separation

In classic clean-room design, one team studies the original system and writes a behavioural specification that captures what it does, not how. A second team, walled off from the original code, implements from that spec alone. Because the implementers never touched the protected expression, their work is not a derivative of it. Phoenix's clean reimplementation of the IBM PC BIOS is the textbook case.

The same wall maps onto agents almost directly. A describer agent aligns with the user on the desired behaviour and produces `spec.md`. An implementer agent runs in a fresh context that receives only `spec.md`. A grader runs the hidden tests. Keeping these in separate contexts is the entire point. A single agent that has read both the original and the tests has already torn the wall down.

## 6.3 Where the hidden tests come from

There are two practical sources.

**(a) From an existing project, as a legal AI-assisted clone.** Say you want a clean reimplementation of something that already exists, to modernize it, drop a proprietary dependency, relicense it, or port it to another stack. One agent observes the original's behaviour and public documentation and writes the spec. A separate agent implements from that spec only. The original's own test suite, or a black-box suite you derive from observed inputs and outputs, becomes the hidden oracle.

The legal care is necessary. The spec and the implementation have to come from behaviour and documentation, not from reading and paraphrasing the source, or the wall is not clean. Test suites and source are themselves copyrightable, so prefer characterization tests generated from observed behaviour over copying the original's tests verbatim.

**(b) From the spec itself, as AI-built tests.** When there is no original to clone, a separate agent reads the spec and writes the acceptance tests, and a different agent implements against them. This is acceptance-test-driven development with an adversarial split: the test author and the code author never share a context.

The trap here is correlated blind spots. If one model writes both the tests and the code from the same spec, a misreading of the spec corrupts both, and the tests will happily pass the wrong implementation. The defences are to use a different model or a deliberately adversarial prompt for the test author, provide a smaller set of human-written golden cases or even apply fuzz testing techniques.

## 6.4 Running the loop without leaking the oracle

Mechanically, the implementer receives the spec, writes code, and runs a command that executes the hidden tests and returns a verdict. It sees the pass and fail counts, perhaps a failing assertion message, but not the test source.

Even a bare pass or fail leaks information over many rounds. Tuning code against a hidden grader, iteration after iteration, is equivalent to overfitting.A few mitigation strategies are:

- Let the agent iterate only against a smaller development slice and grade on the full suite at the end.
- Keep the feedback coarse.
- Rotate or grow the suite between runs.

## 6.5 Doing this with Spec Kit and Superpowers

Spec Kit gives you the inputs for free. `spec.md`, the `contracts/` folder and the acceptance criteria are exactly what a test-authoring agent needs. The catch is that `/speckit.implement` by default has a single agent write and satisfy its own tests, so a clean-room setup means deliberately splitting test generation from implementation into separate agents or runs and withholding the test files from the implementer.

Superpowers gets you most of the wall by accident. Its subagent-driven-development skill already dispatches a fresh subagent per task with an isolated context. One subagent authors the tests from the spec, then the manager dispatches the implementer subagent with the spec and an instruction to make the hidden suite pass, but without the test bodies in its context. After the implementer subagent completes, the spec compliance and code quality reviews are dispatched. The test-driven-development and verification-before-completion skills supply the discipline, and the clean-room separation supplies the independence.

## 6.6 Why it is worth the trouble

Advantages:

1. **Resists test gaming.** The agent cannot pass by memorizing the answers, because it never sees them.
2. **Produces an honest, numeric measure** of how well the code matches the spec, independent of the agent that wrote it.
3. **Makes legal reimplementation tractable,** with the spec serving as the documented wall between the original and the clone.

Limits and risks:

1. **Hidden tests only check what the spec covers.** Underspecified behaviour goes unverified, or gets pinned to whatever arbitrary choice the test author happened to make.
2. **Correlated blind spots** when one model writes both the spec-derived tests and the code.
3. **Oracle overfitting** through repeated feedback, unless you hold part of the suite back.
4. **Genuine legal complexity** in the cloning case. The wall has to be clean, and tests, APIs, trade secrets and licences each carry their own constraints.
5. **Higher cost,** since you are running an isolated describer, implementer and grader instead of one agent doing everything.

---

## 7) The wider landscape (brief survey)

Spec Kit and Superpowers are the two most visible options, but the SDD space is busier than that:

- **Kiro** (AWS). An agentic IDE built around specs from the ground up. You describe a feature and it produces requirements, a design and a task list as first class files, with hooks that react to events in the editor.
- **BMAD-METHOD.** An open framework that models an agile team as a set of agents (analyst, product manager, architect, scrum master) which collaborate to turn an idea into a PRD, an architecture and sharded stories an implementation agent then builds.
- **Tessl.** A spec centric, "AI native" approach where the specification is the primary artifact and code is regenerated from it, paired with a registry of reusable specs.
- [**AGENTS.md](http://agents.md/).** Not a tool but a convention. A single markdown file at the repo root that tells any agent how to work in this codebase. It pairs well with both tools above and is becoming a de facto standard.
- **Agent Skills.** You can write project specific skills without adopting the whole Superpowers library.

The common thread across all of them is the same bet: capture intent in a durable, machine readable artifact and let the agent work from that, rather than from a chat history that vanishes.

---

## 8) References

### 8.1 Spec Kit

1. GitHub. **github/spec-kit**. `https://github.com/github/spec-kit`
2. Delimarsky, D. **Spec-driven development with AI**. The GitHub Blog, September 2025. `https://github.blog/ai-and-ml/generative-ai/spec-driven-development-with-ai-get-started-with-a-new-open-source-toolkit/`

### 8.2 Superpowers

1. Vincent, J. **obra/superpowers**. `https://github.com/obra/superpowers`
2. Vincent, J. **Superpowers**. [blog.fsck.com](http://blog.fsck.com/), October 2025. `https://blog.fsck.com/2025/10/09/superpowers/`

### 8.3 Wider landscape

1. **AWS Kiro**. `https://kiro.dev`
2. **BMAD-METHOD**. `https://github.com/bmad-code-org/BMAD-METHOD`
3. [**AGENTS.md**](http://agents.md/). `https://agents.md`
4. **Agent Skills**. `https://docs.anthropic.com/en/docs/claude-code/skills`

### 8.4 Clean-room engineering and evaluation

1. **Clean room design**. `https://en.wikipedia.org/wiki/Clean_room_design`
2. **Google LLC v. Oracle America, Inc.**, 593 U.S. (2021). `https://www.supremecourt.gov/opinions/20pdf/18-956_d18f.pdf`
3. Feathers, M. **Working Effectively with Legacy Code**. Prentice Hall, 2004. (characterization tests for pinning legacy behaviour)
4. Amodei, D.; et al. **Concrete Problems in AI Safety**. arXiv:1606.06565, 2016. `https://arxiv.org/abs/1606.06565` (reward hacking and specification gaming)
