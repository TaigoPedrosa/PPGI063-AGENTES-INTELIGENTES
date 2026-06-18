# The Contract Net Protocol

> Summary of the first thread of [Intelligent Agents (1990–2000)](pdfs/intelligent-agents-1990-2000.pdf).

The Contract Net Protocol (CNP) is the decade's foundational answer to a deceptively simple
question: **in a system of many autonomous agents, who does what?** Its answer is to let
agents **negotiate** the way a market does.

---

## Context and motivation

The problem CNP set out to solve:

- How do you **distribute tasks** in distributed systems?
- How do agents **decide who executes** a task?
- How do you **coordinate multiple agents autonomously**?

The inspiration came from the **business contract market**: a company publishes a tender
(an *edital*), suppliers submit bids, and the best proposal is selected. **Reid Smith**
turned this everyday idea into a protocol for intelligent systems.

---

## What the Contract Net Protocol is

A **negotiation and coordination protocol** between agents.

- **Goal:** distribute tasks **dynamically** among agents.
- **Participants:**
  - **Manager** — the agent that *owns* the task.
  - **Contractor** — the agent that *executes* the task.

The overall flow is: *Manager announces a task → agents submit proposals → manager picks a
winner → winner executes → contract is concluded.*

---

## The four phases

### Phase 1 — Task announcement

The manager broadcasts a message containing the **task description**, **requirements**,
**deadline**, and **selection criteria**. The goal is simply to **find interested agents**.

> *Example (warehouse robots):* the manager announces *"Who can move 20 boxes to sector B
> in under 10 minutes?"*

### Phase 2 — Bidding

Each agent analyzes its **availability, cost, capability, and estimated time**, then submits
a proposal. Crucially, **bidders do not see each other's bids** (blind competition).

> *Example bids:* Robot A → 8 min; Robot B → 5 min; Robot C → unavailable.

### Phase 3 — Awarding

The manager picks the **best proposal** by criteria such as **lowest cost**, **shortest
time**, **reliability**, or **lowest resource consumption**. The others are told they were
not selected.

> *Example:* Robot B (5 min) wins — shortest time.

### Phase 4 — Contract execution

The winning agent **executes the task**, **sends status updates**, and **reports
completion**. Once the manager receives the conclusion, it **closes the contract** and can
use the result in its own tasks.

> *Example:* Robot B moves the boxes and reports *"Task completed successfully."*

---

## Advantages and disadvantages

| Advantages | Disadvantages |
| --- | --- |
| Decentralization | High message volume |
| Flexibility | No guarantee of a globally optimal solution |
| Scalability | Can be slow in real time |
| Recursive (a contractor can become a manager) | "Blind" competition |
| Better task distribution | Network failures can stop bids reaching the manager |

---

## Real-world applications

CNP fits any system that must **distribute tasks dynamically among multiple autonomous
agents**:

- collaborative robotics, intelligent logistics, the Internet of Things (IoT);
- drones, autonomous vehicles, industrial systems, distributed computing.

Most importantly for this section's story, it **influenced modern agent communication
standards (FIPA)** — and survives today as the **FIPA-Contract-Net** interaction protocol
shipped inside platforms like **JADE**.

---

## Takeaways

- CNP reframed coordination as **negotiation**, a market metaphor that scales without a
  central controller.
- Its **announce → bid → award → execute** cycle is still the canonical pattern for task
  allocation in multi-agent systems.
- It is the first link in this era's chain: negotiation needs a **language** to happen in —
  which the next threads (AOP, KQML, FIPA-ACL) provide.
