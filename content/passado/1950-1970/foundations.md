# Foundations - Early Intelligent Agents (1950–1970)

> Overview page derived from the team's presentation:
> [Agentes Inteligentes no Passado (1950–1970)](pdfs/agentes-inteligentes-1950-1970.pdf) (43 slides).
> Team: Fernando Souza, Ruth Maria, Victor Lemos, Vinicius Sousa.

This section traces the **first two decades of Artificial Intelligence**, from the
mathematical models of the brain in the 1940s–50s to the symbolic systems, planning
robots, and expert systems of the 1960s–70s. It is the historical groundwork on which
every later agent paradigm, the *present* tool-using LLM agents and the *future*
world-model agents, was built.

---

## The central question

The whole era revolves around a single problem:

> **How do we mechanize goal-directed behavior? How do we program *purpose*?**

Two mathematical worldviews emerged to answer it, and the tension between them defines
the period:

- the **continuous flow** of physics (cybernetics, control theory), and
- the **discrete deduction** of logic (symbolic AI).

---

## Timeline at a glance

| Year | Milestone | Why it matters |
| --- | --- | --- |
| **1943** | McCulloch & Pitts, *A Logical Calculus of the Ideas Immanent in Nervous Activity* | First mathematical model of a neuron; networks can compute logic (AND/OR/NOT). |
| **1948** | Wiener, *Cybernetics* | Feedback, control, and communication as the basis of "intelligent" machines. |
| **1950** | Turing, *Computing Machinery and Intelligence* | The Turing Test: behavior, not internal process, as the measure of intelligence. |
| **1956** | Dartmouth Conference | The term "Artificial Intelligence" is coined; symbolic AI is born. |
| **1956** | Logic Theorist (Newell, Shaw, Simon) | First program to prove mathematical theorems; list processing (IPL). |
| **1957–59** | General Problem Solver (GPS) | Means-ends analysis; separates problem from solving strategy. |
| **1965** | DENDRAL | First high-performance expert system; knowledge beats brute-force search. |
| **1966–72** | Shakey (SRI) | First mobile robot to reason about its own actions; STRIPS planner. |
| **1968–70** | SHRDLU (Winograd, MIT) | Natural-language understanding and planning in a blocks-world microworld. |
| **~1970s** | MYCIN | Rule-based medical diagnosis under uncertainty; explainable reasoning. |

---

## The five threads of this section

The presentation is organized into five themes, each with its own summary page:

1. **The Birth of AI and Rationality** — the artificial neuron, the Turing Test, and the
   Dartmouth Conference that named the field.
2. **The Symbolic Era & the General Problem Solver** — Logic Theorist and GPS, the dream
   of a universal, symbol-manipulating problem solver.
3. **Microworlds & the First Planning Robots** — SHRDLU and Shakey reasoning, planning,
   and acting inside simplified, fully known environments; the STRIPS representation.
4. **The Era of Expert Systems** — the epistemological turn: performance depends on
   *knowledge*, not just search. DENDRAL, MYCIN, and XCON.
5. **Control Theory & Classical AI** — cybernetics and feedback versus symbolic
   reasoning, and how their convergence foreshadowed the modern *perceive → plan → act*
   agent.

---

## The thread that connects them

A single insight matures across the two decades:

> General-purpose search alone does not scale to the real world. What makes an agent
> capable is the **quantity and quality of knowledge it has about its domain**, plus an
> architecture that can **perceive, plan, and act**.

This is exactly the lineage that the **Presente** and **Futuro** sections continue:
from symbolic reasoning and hand-coded rules, to learning systems, to today's
language-based agents, and on to multimodal world-model agents.
