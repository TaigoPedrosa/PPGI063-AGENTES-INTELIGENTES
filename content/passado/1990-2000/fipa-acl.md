# FIPA-ACL

> Summary of the fourth thread of [Agentes Inteligentes (1990–2000)](pdfs/agentes-inteligentes-1990-2000.pdf).

KQML proved that agents *could* talk; **FIPA-ACL** is what made that talk an **official
standard** with a **rigorous formal semantics**. It is the language that won — and the one
still implemented by modern agent platforms.

---

## FIPA — Foundation for Intelligent Physical Agents

A **non-profit association** whose goal was to promote **interoperability** between
agent-based applications through **internationally agreed specifications** (Finin & Labrou,
2001).

| Year | Milestone |
| --- | --- |
| **1996** | FIPA founded (international consortium) |
| **1997+** | First ACL specifications |
| **2002** | Consolidated specification |
| **2005** | FIPA becomes an **IEEE** standards committee |

---

## What FIPA-ACL is

- **Based on speech acts** — like KQML, but with **more rigorous formal treatment**.
- **Syntax almost identical to KQML** — lists with a performative and parameters.
- **Formal semantics based on modal logic** — every communicative act is defined by:
  - **Feasibility Preconditions (FPs):** conditions that must hold **before** the agent
    performs the act;
  - **Rational Effect (RE):** the effect the agent **intends to produce**.
- **SL (Semantic Language):** the content language, built on a **BDI modal logic**
  (Belief–Desire–Intention) with operators like `B` (belief), `I` (intention), and `U`
  (uncertainty).

---

## Communicative acts

| Family | Acts |
| --- | --- |
| **Information** | `inform`, `confirm`, `disconfirm`, `subscribe` |
| **Delegation** | `request`, `agree`, `refuse`, `cancel` |
| **Negotiation** | `cfp`, `propose`, `accept-proposal`, `reject-proposal` |
| **Error** | `not-understood`, `failure` |

> The **negotiation** family (`cfp` = call for proposals) is exactly the Contract Net
> Protocol expressed as standard communicative acts.

---

## The formal meaning of a message

A communicative act is defined with modal-logic preconditions and effects:

```
<i, inform(j, φ)>
  FP: Bi φ ∧ ¬Bi (Bif j φ ∨ Uif j φ)
  RE: Bj φ
```

**Read as:** agent **i** informs **j** of **φ**, assuming that:

- **i** believes **φ**;
- **i** does not believe **j** already has an opinion about **φ** (nor any leaning on it).

The **rational effect** intended is that **j comes to believe φ**. This precise, logic-based
definition is what distinguishes FIPA-ACL from KQML's informal semantics.

### A message in FIPA-ACL

```lisp
(inform
  :sender meteo-sensor
  :receiver app-clima
  :content "weather(today, raining)"
  :language Prolog
  :ontology weather-domain
  :reply-with msg-001)
```

The structure mirrors KQML almost exactly — the difference is **underneath**, in the formal
semantics of each act.

---

## KQML vs. FIPA-ACL

| Aspect | KQML | FIPA-ACL |
| --- | --- | --- |
| **Origin** | ARPA KSE (1993) | FIPA (1997–2002) |
| **Syntax** | Lisp-like | Lisp-like (nearly identical) |
| **Semantics** | Pre/post-conditions | BDI modal logic |
| **Content** | Agnostic | Tends toward SL |
| **Facilitation** | Dedicated performatives | Treated as `request` |
| **Standardization** | De facto | IEEE (official) |

---

## Takeaways

- FIPA-ACL took the speech-act idea and gave it **mathematical precision** (FP + RE over a
  BDI logic), removing the ambiguity that limited KQML.
- Backed by an **IEEE standard**, it became the **interoperability baseline** for agent
  platforms.
- Its `cfp / propose / accept-proposal` acts standardize **negotiation** — closing the loop
  back to the Contract Net Protocol and forward to its implementation in **JADE**.
