# Control Theory & Classical AI

> Summary of the fifth thread of [Agentes Inteligentes no Passado (1950–1970)](pdfs/agentes-inteligentes-1950-1970.pdf).

The same era produced **two rival answers** to how machines achieve purposeful behavior:
the **continuous** world of control theory and the **discrete** world of symbolic AI. Their
eventual convergence sketched the first modern agent.

---

## The central problem

> How do we mechanize goal-directed behavior? How do we program *purpose*?

An agent maps **input → mechanism → output**. Two mathematical worldviews emerged to
describe that mechanism: the **continuous flow** of physics and the **discrete deduction**
of logic.

---

## Cybernetics

- Before AI consolidated as a discipline, ideas about intelligent machines came from
  **cybernetics**, popularized by **Wiener's 1948 book**.
- It studied **control systems, communication**, and above all how **feedback**
  (retroaction) mechanisms work.
- In this phase, physical agents were built entirely on **reactions analogous to biological
  reflexes**.

### Feedback / retroaction

The foundation of control theory:

- the system **measures error** (difference between a reference and the measured output),
- and **continuously adjusts** its action to reduce it (a closed control loop:
  *reference → controller → target system → measured output → back to the comparator*).

---

## Classical control theory

- works with **continuous variables**,
- uses **mathematics and differential equations**,
- belongs to **engineering and automation**.

Typical controlled quantities: **temperature, speed, pressure**. The canonical historical
example is **Watt's centrifugal governor** for the steam engine.

---

## The classical AI paradigm

By contrast, the **symbolic AI** born at Dartmouth (term coined by **John McCarthy**, who
criticized cybernetics) bet that *"intelligence requires symbolic reasoning."* Its pillars:

- **symbol manipulation**,
- **formal logic**,
- **inference**,
- **knowledge representation**.

Typical applications: **theorem proving, games, planning**, often expressed as **search over
trees/graphs**.

---

## Control vs. Classical AI

| Control Theory | Classical AI |
| --- | --- |
| Continuous variables | Discrete states |
| Differential equations | Symbolic logic |
| Physical control | Reasoning |
| Reaction to the environment | Planning |

A useful intuition: in a maze, **cybernetics reacts continuously and follows sensors**,
while **classical AI plans and searches for a path**.

---

## Convergence: the first modern agent

Control theory and classical AI **cannot stand alone**, they are **complementary**. Their
combination produced the first picture of how future agents would be structured.

**Shakey (1966–1972)** embodied this convergence with a three-stage loop:

- **Perception**,
- **Planning**,
- **Action**.

> This *perceive → plan → act* loop is the direct ancestor of the modern agent architecture
> revisited in the *Presente* and *Futuro* sections, where perception becomes multimodal and
> planning moves into learned, latent world models.
