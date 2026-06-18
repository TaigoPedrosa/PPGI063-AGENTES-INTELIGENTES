# The Architecture of the Artificial Mind

> Summary of the first thread of [Agentes Inteligentes (1980–1990)](pdfs/agentes-inteligentes-1980-1990.pdf).

The 1980s reframed intelligence as an **agent** and tried to give that agent a principled
**cognitive architecture** grounded in human psychology.

---

## Defining intelligence as an agent

Russell & Norvig, in *Artificial Intelligence: A Modern Approach*, redefine intelligence as
a **dynamic process**: rational action rests on a continuous cycle of **perception through
sensors** and **response through actuators**. This sensor → reason → actuator loop became
the standard lens for understanding the interaction between humans and intelligent
machines, and the backbone of the agent paradigm.

---

## Institutionalization

The field consolidated as an academic discipline:

- The first **AAAI conference (1980)** and the **IJCAI** were milestones in solidifying AI.
- This transition produced **standardized autonomous systems**, globalized knowledge, and
  enabled collaboration among researchers and practitioners worldwide.

---

## Two competing currents

The decade was shaped by a tension between two answers to "how does intelligence work?"

- **Expert systems (top-down):** knowledge bases of **IF–THEN** rules that emulate the
  reasoning of human experts. The approach is to **capture explicit expert knowledge and
  encode it as rules**.
- **Reactive agents (bottom-up):** Rodney Brooks argued intelligence needs **no internal
  world model**, it emerges from the direct coupling of perception and action. His
  **subsumption architecture** decomposes behavior into layers (avoid obstacles, wander,
  explore), where higher layers can suppress lower ones. *(Explored in depth on the
  Production Systems & Reactive Critique page.)*

---

## ACT-R (Adaptive Control of Thought–Rational)

A cognitive architecture that models human cognitive processes based on psychology. Its
central distinction:

- **Declarative memory** — answers the **"what"** (facts).
- **Procedural memory** — answers the **"how"** (skills, actions).

### From facts to automatic skill

The connection between declarative and procedural knowledge is fundamental: **repeating
facts produces automatic actions**, letting the brain automate tasks. This is the basis of
learning and efficiency in everyday activities.

### Biological validation with fMRI

ACT-R's modules were validated using **fMRI** (functional magnetic resonance imaging), a
neuroimaging technique that observes the brain working in real time:

- when ACT-R accesses **declarative memory**, the model predicts activation in the
  **prefrontal cortex**;
- when ACT-R executes a **procedural rule**, the model points to the **caudate nucleus**.

The imaging **confirmed both predictions**, a rare bridge between an AI architecture and
measurable brain activity.

---

## Takeaways

- Intelligence is now an **agent**: perceive, reason, act.
- AI matures into a **standardized, institutional discipline**.
- **ACT-R** shows the era's ambition: not just systems that work, but architectures that
  mirror, and are validated against, **human cognition**, a goal pushed even further by
  **SOAR** (next page).
