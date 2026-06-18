# The Symbolic Era & the General Problem Solver

> Summary of the second thread of [Agentes Inteligentes no Passado (1950–1970)](pdfs/agentes-inteligentes-1950-1970.pdf).

After Dartmouth, the dominant idea was that intelligence is **symbol manipulation**. Two
programs by Newell, Shaw, and Simon defined the era.

---

## Logic Theorist (1956)

A program able to **prove mathematical theorems**.

- Demonstrated that machines could **manipulate symbols**, not just crunch numbers.
- Introduced the concept of **list processing** through **IPL** (Information Processing
  Language).
- Worked by exploring **search trees**.
- Presented at the **Dartmouth Conference**.

> Often called the first true AI program: it proved several theorems from *Principia
> Mathematica*.

---

## General Problem Solver (GPS, 1957–1959)

Designed to be a **universal problem-solving machine**.

- In principle, any problem that could be expressed as a set of **well-formed formulas
  (WFFs)** could be solved by GPS.
- Used **means-ends analysis**: it generated heuristics by comparing the current state to
  the goal and creating **sub-goals** to close the gap, in a loop.
- It was the **first program to separate the problem from the solving strategy** — a
  general engine decoupled from any specific domain.
- It focused on the **available operators**, finding which inputs were acceptable and
  which outputs they produced, then chaining them toward the goal.

### Where it broke down

- GPS could **not solve real-world problems**: the search got lost in **combinatorial
  explosion**.
- Solving toy problems (e.g., the Tower of Hanoi) was feasible; scaling to large or
  open-ended state spaces was not.

---

## Takeaways

- The symbolic era proved that **reasoning could be automated** as search over symbols.
- **Means-ends analysis** and the **problem/strategy separation** are lasting
  contributions, still visible in modern planning.
- The fatal limitation, **combinatorial explosion in large state spaces**, is the same
  structural problem that motivates knowledge-based systems (next thread) and, much later,
  learned planning in the *Futuro* section.
