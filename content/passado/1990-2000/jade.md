# JADE — Java Agent Development Framework

> Summary of the fifth thread of [Agentes Inteligentes (1990–2000)](pdfs/agentes-inteligentes-1990-2000.pdf).

JADE is where the whole decade lands: an **open-source platform** that takes FIPA-ACL,
speech acts, and the Contract Net Protocol and turns them into a **running, distributed
multi-agent system**. It is the theory of the 1990s, made executable.

---

## What JADE is

An **open-source platform** for **peer-to-peer**, agent-based applications.

- **FIPA compliance** — written entirely in **Java**, JADE is a **middleware** that
  implements the **Foundation for Intelligent Physical Agents** specifications.
- **Interoperability** — a development framework for multi-agent systems that conform to
  FIPA.
- **Free software** — distributed under the **LGPL** license, easing both academic and
  commercial use.

---

## Distributed architecture

Unlike centralized systems, JADE runs on a **distributed infrastructure across multiple
JVMs** (Java Virtual Machines).

- **Containers** — each JADE instance is a **container** that hosts agents.
- **Main-Container** — the platform's entry point, which **must** host:
  - **AMS (Agent Management System):** controls the agent **life cycle** and provides the
    **White Pages** service (name and address);
  - **DF (Directory Facilitator):** the **Yellow Pages** service, where agents register and
    look up services by type;
- **ACC and MTP** — the communication channel (**ACC**) uses message transport protocols
  (**MTP**). This lets JADE communicate with **other platforms** via protocols like **IIOP
  (CORBA)**, crossing network and system boundaries.

---

## Core services

### White Pages (AMS)

Provided by the **Agent Management System**. There is exactly **one AMS per platform**, and
it holds supervisory control over access to and use of the whole system.

- **What it does:** the official directory of **Agent Identifiers (AIDs)** and each agent's
  life-cycle state.
- **How it works:** every agent entering the system must **register with the AMS** to obtain
  a valid AID — a unique identity and transport address for communication.
- **Automation in JADE:** the framework calls the register/deregister methods automatically
  during the agent's `setup()` and `takeDown()` methods.

### Yellow Pages (DF)

Provided by the **Directory Facilitator**.

- **What it does:** a **catalog** where agents publish the services they offer, or search for
  agents providing services they need.
- **How it works:** through a helper class (**DFService**), agents can register, deregister,
  modify, and search service descriptions in the DF catalog.
- **Directory federation:** multiple DFs can be **federated**, building complex networks of
  Yellow-Pages domains and subdomains.

---

## Agent life cycle (states)

The agent life cycle is managed by the AMS. The possible states:

- **Initiated** — object instantiated, but with no formal identity yet.
- **Active** — registered and executing tasks.
- **Suspended** — execution paused (thread suspended).
- **Waiting** — blocked, awaiting an event (e.g., a message in the queue).
- **Deleted** — registration removed and thread destroyed; cleanup runs in `takeDown()`.
- **Transit** — exclusive to **mobile agents**; uses JADE's mobility ontology to migrate
  state and code between containers.

---

## Behaviours — the task model

| Behaviour type | Technical description |
| --- | --- |
| **OneShotBehaviour** | A single, atomic execution. |
| **CyclicBehaviour** | Runs repeatedly; must use `block()` logic so it does not saturate the CPU. |
| **Waker / Ticker** | Time-based scheduling (a single delay, or periodic execution). |
| **Composite** | Complex structures: **Sequential** (step by step), **Parallel** (logical internal concurrency), or **FSM** (finite-state machine). |

---

## Communication (ACL and the `jade.proto` package)

Communication is **asynchronous**, based on message passing compliant with the **ACL**.

- **Privacy:** each agent has a **private message queue**, inspired by the **Actor Model**
  (Hewitt / Agha).
- **Ready-made protocols** — the `jade.proto` package offers robust interaction patterns:
  - **AchieveRE (Achieve Rational Effect):** a generic pattern used to implement the
    **FIPA-Request** and **FIPA-Query** protocols;
  - **FIPA-Contract-Net:** the **negotiation** protocol for distributing tasks among
    multiple proposers — the Contract Net Protocol, shipped as a reusable component.

---

## Demo

The deck closes with a live demo: a **Cliente** (InitiatorAgent) running the
**FIPA-Contract-Net** protocol against three **Transportadora** responder agents
(`CFP → PROPOSE/REFUSE → ACCEPT/REJECT → INFORM`), all inside a Main-Container on a Docker
host, with system agents (**AMS**, **DF**, **RMA**) and tool agents (**Sniffer** for
FIPA-ACL monitoring, **Dummy Agent** for manual tests).

---

## Takeaways

- JADE is the **synthesis of the decade**: FIPA-ACL communication, White/Yellow Pages
  services, behaviours, and the Contract Net Protocol, all in one runnable platform.
- Its **distributed, FIPA-compliant** design made interoperable multi-agent systems
  *practical*, not just theoretical.
- The demo closes the arc that opened with Reid Smith's tendering metaphor: **negotiation as
  a deployable, standardized building block**.
