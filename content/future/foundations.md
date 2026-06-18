# Foundations - Future Agent Principles

This page provides the conceptual bridge between the **present** and **future** sections of the project.  
While present-day intelligent agents are mostly language-based, tool-using, and software-centered, future agents are expected to become increasingly **multimodal**, **world-grounded**, and capable of **long-horizon planning** under real-world constraints.

Rather than replacing the present-agent paradigm, this future-oriented view extends it: from agents that operate mainly through text, APIs, and digital tools to agents that can **perceive**, **model**, **predict**, and **act** in complex environments.

---

## From Present Agents to Future Agents

Present intelligent agents are primarily optimized for **digital environments**.  
They perform well in coding, web interaction, API orchestration, file manipulation, and software automation.

Future intelligent agents are expected to go beyond this paradigm by incorporating:

- **multimodal perception** (vision, video, spatial signals, sensor data),
- **world models** that represent and predict environment dynamics,
- **memory and internal state tracking**,
- **planning under uncertainty**,
- **action under physical or real-world constraints**.

The key transition is therefore:

> from **software-centered agents**  
> to **agents that can understand, simulate, and plan in the world**.

---

## Guiding Principles for Future Agents

The following principles summarize the main lessons from the papers and presentations collected in this section:

- Prefer **world-grounded** architectures over language-only agents for tasks involving physical or dynamic environments.
- Separate the agent stack into **perception**, **world modeling**, and **planning** layers whenever possible.
- Treat **multimodality** as a core capability, not as a secondary add-on.
- Use **hierarchical planning** for long-horizon tasks or tasks requiring intermediate subgoals.
- Align the **level of autonomy** with the cost of failure and the need for human oversight.
- Consider **safety, governance, and uncertainty** as first-class design constraints.
- Balance **capability and compute efficiency**, especially for agents intended to operate continuously or at scale.

---

## Conceptual Shift: from Tools to World Models

A useful way to understand the evolution of intelligent agents is to compare the dominant logic of current systems with the emerging logic of future systems.

### Present-day agents
- Language-based
- Tool-using
- Software-centered
- Strong in digital environments
- Limited perception of the physical world
- Usually reactive or prompt-driven

### Future agents
- Multimodal
- World-grounded
- Planning-oriented
- Able to represent environment state
- Better suited for long-horizon decision-making
- More relevant for robotics, navigation, simulation, and physical AI

This shift does not mean that tools and language become irrelevant.  
Instead, it means that they could no longer be sufficient on their own for the next generation of intelligent agents.

---

## Why World Models Matter

A central idea in this section is that **LLMs predict the next word, while world models predict the next state of the world**.

This distinction matters because intelligent agents that interact with real environments need more than fluent language generation. They need internal mechanisms to:

- represent the current state of the environment,
- anticipate the consequences of actions,
- evaluate possible futures,
- and plan sequences of actions toward a goal.

World models are therefore promising because they connect:

- **perception** → understanding the environment,
- **prediction** → simulating what may happen next,
- **planning** → choosing better actions before acting.

---

## Open Challenges

Although future-agent architectures are promising, several core problems remain unsolved:

- **Long-horizon planning:** prediction errors accumulate over time, making long sequences difficult.
- **Uncertainty-aware modeling:** many systems still struggle to represent uncertainty explicitly.
- **Persistent memory:** agents need better mechanisms to store and reuse past experience.
- **Sim-to-real transfer:** performance in simulation does not always generalize to the real world.
- **Scalability and compute cost:** physical-world agents may require large amounts of data and computation.
- **Safety and governance:** higher autonomy increases the need for control, accountability, and risk management.

These challenges show that the future of intelligent agents is not just a matter of scaling current models, but of developing better architectures for perception, world modeling, and planning.

---

## Representative Direction of Progress

Across the materials in this section, a broad progression appears:

1. **Perception improvement**  
   Better visual and multimodal representations of the world  
   *(e.g., V-JEPA 2.1)*

2. **World modeling**  
   Learning compact predictive models of environment dynamics  
   *(e.g., LeWorldModel)*

3. **Planning improvement**  
   Structuring search and control in latent or hierarchical forms  
   *(e.g., Temporal Straightening, HWM)*

4. **Deployment toward physical AI**  
   Extending agent intelligence to embodied, distributed, and real-world settings

This sequence provides a useful conceptual map for understanding the future of intelligent agents.

---

## Conclusions

The future of intelligent agents is likely to depend on a transition from **language-only, tool-centered software agents** to **multimodal agents that can model, predict, and plan in the world**.

In this view:

- perception becomes richer,
- internal models become more predictive,
- planning becomes more hierarchical,
- and autonomy becomes more closely tied to safety, governance, and real-world constraints.

This page serves as the conceptual foundation for the more detailed materials in:

- **Articles - World Models & Agentic AI**
- **Best Practices - Future Agents & World Models**
- **Experiments - World Model Benchmarks**
- **Costs - Compute & Efficiency**