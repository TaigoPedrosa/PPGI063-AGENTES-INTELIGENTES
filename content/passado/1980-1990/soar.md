# The SOAR Cognitive Architecture

> Summary of the second thread of [Agentes Inteligentes (1980–1990)](pdfs/agentes-inteligentes-1980-1990.pdf).

SOAR is the decade's most ambitious attempt at a **unified theory of cognition**, a single
architecture meant to explain how minds think, solve problems, and learn.

---

## Historical context

- Created in **1983** at **Carnegie Mellon University**.
- Developed by **Allen Newell, John Laird, and Paul Rosenbloom**.
- **Goal:** build a **unified model of human cognition** capable of simulating how the human
  mind **thinks, solves problems, and learns from experience**.

---

## The core idea

SOAR models human reasoning as **search in a problem space**:

- define an **initial state**,
- apply **operators** (possible actions),
- try to reach a **goal state**.

### How reasoning works

A worked example, a robot that must move a box:

- **State** — the agent's current situation (*robot in room A, box in room B, goal: move
  the box*).
- **Operators** — the possible actions (*move to room B, pick up box, push box*).
- **Operator selection** — the system chooses which operator to apply using **production
  rules** (`IF condition THEN action`).
- **Action application** — the operator runs and the problem state changes.

The cycle repeats until the goal is reached.

---

## Chunking: learning from experience

**Chunking** is SOAR's automatic learning mechanism. When the system solves a hard problem
it:

1. analyzes the reasoning it used,
2. creates a **new rule**,
3. stores that rule for future use.

The system effectively learns **cognitive shortcuts**: the first time it solves a problem
step by step (step 1 → 2 → 3 → 4); after chunking, a single learned rule solves it
directly, making the system **faster and more efficient**.

---

## Architecture: three memories

- **Working Memory** — holds the current state of the problem.
- **Production Memory** — holds the IF–THEN rules used to make decisions.
- **Learning Memory** — stores the new rules created by chunking.

---

## Applications

- **Academic:** studies of human cognition, modeling of problem solving, intelligent-agent
  experiments.
- **Military:** strategic planning, decision making, combat simulations.

---

## Why SOAR matters

- proposes a **unified theory of cognition**,
- connects **symbolic AI with cognitive psychology**,
- introduces **automatic, experience-based learning** (chunking).

It influenced research in **intelligent agents, cognitive architectures, and decision
systems**, and shares its production-rule DNA with the **BDI** and **production-system**
threads of this same era.
