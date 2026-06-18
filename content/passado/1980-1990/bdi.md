# BDI (Belief–Desire–Intention)

> Summary of the third thread of [Agentes Inteligentes (1980–1990)](pdfs/agentes-inteligentes-1980-1990.pdf).

BDI gave software agents a model of **practical reasoning** borrowed from how humans
explain their own behavior, and, through PRS, took it all the way to the **Space Shuttle**.

---

## The model

- Developed by the philosopher **Michael Bratman in 1987**.
- Grounded in **folk psychology**, our intuitive ability to understand and explain human
  behavior.
- It is an **architecture for software agents** that simulates human reasoning in
  **decision-making**.

### The three mental attitudes

- **Belief** — what the agent knows or believes about the current state of its environment
  and itself.
- **Desire** — the goals or future states the agent would like to achieve.
- **Intention** — the desires the agent has **committed to pursuing**, carried out through
  **predefined plans**.

---

## The reasoning loop

BDI runs a continuous cycle: **beliefs → reasoning → desires → intentions → action**, then
back again as the world changes. The deck illustrates it with a cleaning robot:

1. **Perception & beliefs** — sensors produce beliefs (*battery 90%, kitchen dirty, living
   room clean*).
2. **Generating desires** — desires compete (*clean the kitchen* vs. *stay charged* vs.
   *explore*).
3. **Intention & action** — one intention is chosen (*clean the kitchen*) with a defined
   action plan.
4. **Update & new intention** — a belief changes (*battery 15%*), aborting the previous
   intention in favor of a more urgent one (*recharge*).

---

## Characteristics

- **Error recovery** — if an intention fails (e.g., a robotic arm jams), the agent
  re-evaluates its beliefs and immediately selects an alternative plan.
- **High-level abstraction** — engineers program **complex goals** instead of endless
  if-else code.
- **Adaptability** — behavior shifts when the environment becomes dangerous, prioritizing a
  "safety" desire over a "production" desire.

## Challenges

- **Real-time** — the deliberation cycle (think before acting) must be fast enough not to
  delay physical processes.
- **Formal verification** — in critical systems, one must **mathematically prove** the
  agent will never make a catastrophic decision.
- **Resource consumption** — BDI agents tend to need more processing than traditional
  programmable logic controllers (PLCs).

---

## PRS (Procedural Reasoning System)

- Developed in the **1980s**, PRS was one of the first frameworks to turn BDI **theory into
  applicable software**.
- **Real-time focus:** instead of building a complete plan before acting, PRS **interleaves
  planning with execution**, ideal for dynamic, real-time environments.
- **Flexible:** it both pursues goals and reacts proactively to unanticipated changes.

Its architecture chains: **input (events/perceptions)** → **database (beliefs)** + **KA
library (plans)** → **interpreter** (selects an applicable Knowledge Area against current
**goals**) → **intention structure** (execution queue) → **action** commands.

### PRS on NASA's Space Shuttle

- PRS's most famous application: a **fault-detection system** for the **Reaction Control
  System (RCS)** of the Space Shuttle **Discovery**.
- It acted as a **human assistant**, reasoning over and controlling diagnostics in real
  time, with **100+ Knowledge Areas**.
- It managed continuous sensor readings and multiple alarms, diagnosing critical failures
  (regulator faults, leaks, component blockages) and beginning **damage containment almost
  instantly**, prioritizing critical actions while still diagnosing causes.

> The takeaway: intelligent agents can **coexist with rigorous safety protocols**, turning
> automation into an entity that can "reason" about the health of a process.

---

## Takeaways

- BDI is the era's most durable **agent abstraction**, still used in multi-agent systems
  today.
- **PRS** proved BDI works under hard **real-time, safety-critical** constraints.
- Its reliance on plans and explicit reasoning places it firmly on the **top-down
  (symbolic)** side of the decade's central debate, the side Brooks would attack next.
