# The Era of Expert Systems

> Summary of the fourth thread of [Agentes Inteligentes no Passado (1950–1970)](pdfs/agentes-inteligentes-1950-1970.pdf).

The limits of general search forced a conceptual shift: stop chasing a universal solver and
start **encoding expert knowledge** about a specific domain.

---

## The epistemological turn

Through the 1950s and early 1960s, much AI research focused on **general search algorithms**
(like the General Problem Solver) exploring state spaces with heuristics. Applied to real
problems, these methods hit a wall: state spaces were **extremely large or potentially
infinite**, making complete or even heuristic search infeasible.

This led to a fundamental change of view:

> An intelligent agent's performance depends **less on the search algorithm itself** and
> **more on the quantity and quality of knowledge** it has about the problem domain.

This insight, the **epistemological turn**, moved the focus from pure reasoning to the
**explicit representation of knowledge**.

---

## The production architecture

From this shift came the **production-system architecture**, with three main components:

- **Knowledge Base** — the domain rules and facts.
- **Inference Engine** — applies the rules, using
  - **forward chaining** (data-driven reasoning), and
  - **backward chaining** (goal-driven reasoning).
- **Working Memory** — the current state of facts during a session.

It also created a new interdisciplinary field, **knowledge engineering**, concerned with:

- extracting tacit knowledge from human experts,
- formalizing it into computational structures,
- validating the rules and inferences the system generated.

---

## DENDRAL (Stanford, 1965) — the first high-performance expert system

- Helped chemists determine the **molecular structure of organic compounds** from
  **mass-spectrometry** data.
- A genuinely hard problem: one molecular formula can map to **thousands** of possible
  structures.

**Conceptual contribution** — DENDRAL demonstrated that:

- specialized knowledge can turn an **intractable** problem into a **computationally
  viable** one,
- AI could **exceed human performance** on highly specialized tasks.

It was also recognized **outside computer science** (by chemists), an early sign that AI
could create real scientific value in other disciplines.

---

## MYCIN (Stanford, early 1970s) — reasoning under uncertainty

- Aimed to **diagnose bacterial blood infections** and recommend the right **antibiotic**
  and **dosage**.
- Unlike DENDRAL's structural chemistry, medicine deals with **incomplete, imprecise, or
  uncertain** information: symptoms overlap, tests are inconclusive, histories are partial.

It used a base of **hundreds of medical rules**, for example:

> IF the patient has a fever AND the bacterium is gram-positive
> THEN there is a probability of *Streptococcus* infection.

### Conversational, explainable operation

MYCIN ran **interactively**, asking the physician questions such as *"Does the patient have
a fever?"* or *"What was the Gram-stain result?"*. Each answer updated its **working
memory**, letting the inference engine generate new diagnostic hypotheses, and the system
could **explain** its reasoning, an early example of explainable AI.

---

## Legacy: XCON and commercial impact

MYCIN's success inspired **commercial** expert systems. A notable example was **XCON**,
built by **Digital Equipment Corporation** to automatically configure computer orders. It
saved an estimated **US$40 million per year**, proving AI could produce **direct economic
impact**.

---

## Limitations that ended the era

- **Knowledge-acquisition bottleneck** — turning human expertise into rules was hard:
  knowledge is often **intuitive**, heuristics are **implicit**, and both are **difficult
  to formalize**.
- **Brittleness** — systems worked only within their intended domain; small changes broke
  them, and they had **low generalization**.

> These limitations pushed the field toward the next paradigm: **Machine Learning**, systems
> that acquire knowledge from data instead of hand-coded rules.
