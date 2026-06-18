# Agent Communication & KQML

> Summary of the third thread of [Intelligent Agents (1990–2000)](pdfs/intelligent-agents-1990-2000.pdf).

This is the conceptual core of the decade: the moment **communication becomes the defining
property of an agent**. It runs from the engineering problem of the 1990s, through the
philosophy of speech acts, to **KQML** — the first widely used Agent Communication Language
(ACL).

---

## The problem of the 1990s

Software had become **distributed and heterogeneous**:

- different **platforms, formats, and languages**;
- a need for **cooperation**, not just data transport.

> **How do you make programs *truly* talk to each other?**

Moving bytes between machines was a solved problem. Making programs **understand and
cooperate** was not.

---

## Genesereth's vision

Genesereth & Ketchpel (*CACM*, 1994) made communication the **criterion of agency**:

> *"An entity is only considered a software agent if it is able to communicate correctly
> using an Agent Communication Language (ACL)."*

The consequences:

- communication stops being a feature and becomes a **test of agency**;
- **interoperability** turns into a fundamental requirement;
- **standardizing the ACL** becomes a priority.

---

## Philosophical base: speech act theory

The foundation comes from the philosophy of language:

> *"To speak is to act. Utterances do not merely describe the world; they perform actions in
> the world."* — J. L. Austin, *How to Do Things with Words* (1962)

Austin's three layers of any utterance:

- **Locution** — uttering the statement;
- **Illocution** — the **intention** (to ask, to assert, to promise);
- **Perlocution** — the **effect** on the listener.

### Three linguistic layers agents must share (Finin et al., 1994)

- **Common syntax** — a shared message format. With no universally accepted language yet,
  agents must use the *same* language or **inter-translatable** ones.
- **Shared semantics (ontology)** — a common framework of concepts so agents can interpret
  message *content*. Understanding the words is not enough; they must **share the meaning**
  of domain terms (planning, biology, finance, …).
- **Pragmatics** — knowing **whom to talk to**, how to find the interlocutor, and how to
  start and maintain a conversation.

---

## KQML — Knowledge Query and Manipulation Language

KQML is the first practical ACL:

- a **message format** with a **Lisp-like syntax**;
- a **protocol for exchanging knowledge at runtime**;
- **content-agnostic** — it does not dictate the language used *inside* the message.

### Three-layer architecture

- **Communication layer** — sender, receiver, message IDs;
- **Message layer** — the performative + its parameters;
- **Content layer** — the actual payload (KIF, Prolog, SQL, …).

### KQML performatives

| Category | Examples |
| --- | --- |
| Query | `ask-one`, `ask-all`, `evaluate` |
| Response | `reply`, `sorry` |
| Informational | `tell`, `untell`, `achieve` |
| Streams | `stream-all`, `next`, `discard` |
| Capabilities | `advertise`, `subscribe`, `monitor` |
| Facilitation | `broker`, `recruit`, `recommend` |

### The meaning of a message: `tell(A, B, X)`

A KQML performative is defined by pre- and post-conditions on mental state:

- **Meaning:** *A asserts to B that A believes X is true.*
- **Precondition in A:** A believes X.
- **Precondition in B:** B wants to know whether A believes X.
- **Postcondition in B:** B knows that A believes X.

### A message in KQML

```lisp
(tell
  :sender meteo-sensor
  :receiver app-clima
  :content (weather today raining)
  :language KIF
  :ontology weather-domain
  :reply-with msg-001)
```

- `tell` → the **performative**;
- `:sender` / `:receiver` → who sends / receives;
- `:content` → what is being said (here, written in **KIF**);
- `:language` → the language of the content;
- `:ontology` → the shared vocabulary;
- `:reply-with` → a message identifier, in case B wants to reply.

---

## Takeaways

- The decade reframes an agent as **fundamentally a communicating entity**.
- **Speech act theory** gives that communication a principled basis: messages are *actions*
  with intentions and effects, not mere data transfers.
- **KQML** is the first working ACL — but its **informal, pre/post-condition semantics**
  left room for the more rigorous standard that follows: **FIPA-ACL**.
