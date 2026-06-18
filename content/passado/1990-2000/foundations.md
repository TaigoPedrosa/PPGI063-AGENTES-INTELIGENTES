# Foundations - Intelligent Agents (1990–2000)

> Overview page derived from the team's presentation:
> [Agentes Inteligentes (1990–2000)](pdfs/agentes-inteligentes-1990-2000.pdf) (46 slides).
> Team: Fernando Souza, Ruth Maria, Victor Lemos, Vinícius Sousa.

If the **1950–1970** era invented symbolic AI and the **1980s** built the *single* agent's
mind, the **1990s** is when agents became **social**. The decade's question is no longer
"how does one agent think?" but **"how do many agents coordinate, negotiate, and talk to
each other?"** It is the era of **multi-agent systems**, **agent communication languages**,
and the first **standards and platforms** that made interoperable agents real.

---

## The central shift: from cognition to communication

The defining quote of the decade reframes what an agent even *is*:

> *"An entity is only considered a software agent if it is able to communicate correctly
> using an Agent Communication Language (ACL)."*
> — Genesereth & Ketchpel, *CACM*, 1994

Communication stops being a feature and becomes the **criterion of agency**. Behind this
lies a concrete engineering problem of the 1990s: software was now **distributed and
heterogeneous**, running across different platforms, formats, and languages. Moving bytes
between them was solved; making them **cooperate** was not.

---

## The five threads of this era

The presentation is organized into five themes, each with its own summary page:

1. **The Contract Net Protocol** — Reid Smith's protocol for **dynamically distributing
   tasks** among agents through an announce → bid → award → execute cycle, modeled on
   business tendering. The foundation of negotiation in multi-agent systems.
2. **Agent-Oriented Programming (AOP)** — Yoav Shoham's (1989) paradigm that constrains OOP
   into agents with a **mental state** (beliefs, capabilities, commitments) that communicate
   through **speech acts**, implemented in the **AGENT-0** language.
3. **Agent Communication & KQML** — the philosophical base (Austin/Searle's speech acts),
   the three linguistic layers agents must share, and **KQML**, the first widely used ACL.
4. **FIPA-ACL** — the international standard that gave agent communication a **formal modal
   semantics** (feasibility preconditions + rational effect) and won out over KQML.
5. **JADE** — the **Java Agent Development Framework**, the open-source, FIPA-compliant
   middleware that turned all this theory into a running multi-agent platform.

---

## The through-line of the decade

| Problem of the 1990s | Answer the decade produced |
| --- | --- |
| How to distribute tasks among autonomous agents? | **Contract Net Protocol** (negotiation) |
| How to model an agent as a *social* software unit? | **Agent-Oriented Programming** (mental state) |
| How can heterogeneous agents understand each other? | **Speech acts + KQML** (a shared ACL) |
| How to standardize that so anyone interoperates? | **FIPA-ACL** (IEEE standard) |
| How to actually build and deploy it? | **JADE** (platform) |

Notice how each thread depends on the previous one: AOP needs speech acts to talk; KQML and
FIPA-ACL formalize those speech acts; JADE implements FIPA-ACL and ships the Contract Net
Protocol as a ready-made interaction. The decade closes with agents that are **autonomous,
communicative, and interoperable by standard** — the direct ancestors of today's
multi-agent systems.

---

## Where this sits in the timeline

- **1950–1970:** symbolic AI is born — search and knowledge.
- **1980–1990:** cognitive architectures, the agent abstraction, and the symbolic-vs-reactive split.
- **1990–2000 (this era):** multi-agent systems, agent communication languages, standards, and platforms.

> See the **Timeline** page for the full 1950→2000 arc, and the **Presente** section for
> where language-based agents go next.
