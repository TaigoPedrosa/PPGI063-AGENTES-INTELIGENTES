# Agent-Oriented Programming (AOP)

> Summary of the second thread of [Intelligent Agents (1990–2000)](pdfs/intelligent-agents-1990-2000.pdf).

Proposed by **Yoav Shoham in 1989**, Agent-Oriented Programming (AOP) is a **specialization
of Object-Oriented Programming (OOP)**. Where an object is a blank slate, an agent is an
object **constrained into a mind** — one that holds beliefs, makes commitments, and talks
to others through speech acts.

---

## From object to agent — the evolution of the paradigm

AOP draws on two philosophical sources:

- Daniel Dennett's **"intentional stance"** — treating a system *as if* it has beliefs and
  intentions;
- John Searle's **speech act theory** — utterances *do things* in the world.

The key idea: **AOP is a restriction of OOP that imposes order**. An object is a *"blank
slate"* where you can store any data; an agent must follow a **rigid psychological model**
so it can **interact socially** with other agents.

### OOP vs. AOP

| Characteristic | OOP | AOP |
| --- | --- | --- |
| **Basic unit** | Object (data + methods) | Agent (autonomous + intentional) |
| **Local state** | Instance variables (arbitrary) | Mental states (beliefs, desires, commitments) |
| **Processing** | Method invocation (imperative) | Capabilities (goal-driven plans) |
| **Communication** | Command message (passing control) | Speech acts (inform, request, promise) |

---

## The components of mental state (B-C-C)

An AOP agent's state is built from:

- **Beliefs** — the agent's knowledge about the world and other agents. They may be
  imprecise or outdated: the agent *believes*, it does not necessarily *know*.
- **Capabilities** — the set of actions the agent can perform to change the world or its own
  belief base.
- **Commitments** — promises of **future action**. An agent commits to another (or to
  itself), and the commitment **persists until fulfilled or rendered impossible**.
- **Decisions** — behavior is dictated by **rules** relating current beliefs and prior
  commitments to select the next action.

---

## Communication as speech acts

In OOP, an object *commands* another to run a method. In AOP, communication is **informative
or requestive**, never a transfer of control:

- **Inform** — updates the **beliefs** of the receiver.
- **Request** — tries to influence the **commitments** of the receiver.
- **Promise** — creates a new **commitment** in the *sender* toward the receiver.

The receiving agent **decides for itself**, based on its own beliefs and rules, whether to
accept a request — **preserving its autonomy** with respect to the sender. This is the heart
of agency: an agent is not commanded, it is *persuaded*.

---

## Legacy and languages (AGENT-0)

- **AGENT-0** was the **first agent-based programming language**, created by Shoham to
  implement these concepts.
- Shoham's work **paved the way for the BDI** (Belief–Desire–Intention) model now widely
  used in complex systems and simulations.
- AOP **did not replace OOP**. It provided a **higher-level abstraction** for managing
  complexity in **distributed, social, and dynamic systems**, where autonomy and social
  interaction matter more than the bare execution of algorithms.

---

## Takeaways

- AOP recast the agent as a **disciplined object with a mind**, connecting the 1980s BDI
  ideas to a concrete programming paradigm.
- Its insistence on **speech-act communication** sets up the rest of the decade: KQML and
  FIPA-ACL are precisely the standardized speech-act languages AOP called for.
- The lineage **AGENT-0 → BDI → modern multi-agent systems** runs straight through this
  page.
