# Microworlds & the First Planning Robots

> Summary of the third thread of [Agentes Inteligentes no Passado (1950–1970)](pdfs/agentes-inteligentes-1950-1970.pdf).

To make symbolic reasoning tractable, researchers shrank the world. **Microworlds** are
simplified, fully known environments where agents could reason, plan, understand language,
and act.

---

## What microworlds are

Artificial, simplified environments used to study AI.

**Main characteristics:**

- a small number of objects,
- simple interaction rules,
- a **fully known** environment,
- a limited set of actions.

**Cognitive processes they let researchers study:**

- reasoning,
- planning,
- language understanding,
- problem solving.

The two landmark examples were **SHRDLU** and the robot **Shakey**.

---

## SHRDLU (Terry Winograd, MIT, 1968–1970)

A system that let a computer **understand natural-language commands** and **manipulate
objects** in a virtual environment.

### The Blocks World

The microworld contained blocks, pyramids, cubes, and cones. The user could issue commands
such as:

- *"put the red block on the green cube"*
- *"pick up the big block"*

Internally, tasks were carried out as sequences of primitive actions, e.g.
`PICKUP(C)` → `PUTDOWN(C)` → `UNSTACK(B,A)` → `STACK(C,A)` → `STACK(B,D)`.

### What SHRDLU could do

- interpret commands in **natural language**,
- **manipulate objects** in the environment,
- **answer questions** about the state of the world,
- **keep context** across a dialogue.

---

## Shakey (Stanford Research Institute, 1966–1972)

Considered the **first mobile robot able to reason about its own actions**.

It combined several technologies:

- **computer vision**,
- **automatic planning**,
- **symbolic representation** of the environment,
- **execution of physical actions**.

It operated in an environment of **rooms, corridors, ramps, and boxes**, and could accept
commands like *"push the block off the platform."*

### The STRIPS algorithm

Shakey planned with **STRIPS**, which represented the world in symbolic logic and described
each action with three elements:

- **preconditions** — what must hold before the action,
- **action** — the operation itself,
- **effects** — what changes after the action.

> STRIPS became one of the most influential representations in automated planning, still
> referenced in modern planning languages.

---

## Why microworlds mattered

SHRDLU and Shakey could **understand instructions, plan actions, and interact with their
environments**. Those capabilities became the foundations of modern fields such as:

- autonomous robotics,
- intelligent agents,
- automated planning,
- language-based AI systems.

Their key weakness, total reliance on a **small, fully known world**, is precisely the gap
that the *Futuro* section's world models aim to close for messy, partially observable
environments.
