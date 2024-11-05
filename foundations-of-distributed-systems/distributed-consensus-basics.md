# Distributed Consensus Basics

In our distributed system, each chef operates independently, handling their assigned meal. But what if the chefs need to make a collective decision—say, selecting the main course for a large event? In such cases, they must reach an agreement on a shared decision. This agreement across multiple chefs (or nodes) is known as **consensus**.

Consensus is essential for distributed systems because it helps ensure that everyone, or every node, agrees on the state of the system. Without it, nodes might have conflicting data, and we would end up with multiple “versions” of the truth, leading to confusion and inconsistency.

Let’s explore why consensus is needed and how it’s achieved in distributed systems.

### Why Do We Need Consensus?

Imagine our chefs decide on the main course independently, without consulting each other. One chef might prepare pasta, while another starts on steak, and yet another on a vegetarian option. At the end of the day, we would have a mix of dishes that don’t align with a unified menu, leading to chaos in the kitchen.

In distributed systems, consensus prevents these mismatches. It ensures that all nodes—or chefs—agree on a single outcome, providing a consistent state across the system.

### Basic Consensus Models

Distributed consensus can be achieved in several ways. Here are a few foundational models:

1. **Leader Election**: Nodes choose a leader who makes the final call for everyone. In our analogy, one chef would act as head chef, deciding on the main course, and the others would follow their lead.
2. **Majority Voting**: Each chef suggests an option for the main course, and the option with the most votes wins. This way, all chefs get a say, and the majority’s choice becomes the agreed-upon decision.
3. **Two-Phase Commit**: Imagine a head chef coordinating with others by asking, “Are we all ready to proceed with pasta?” Once every chef signals readiness, they all move forward with the chosen dish. This model is widely used in databases to ensure consistency.

These models give distributed systems a way to achieve a shared view and handle conflicting opinions or states gracefully.
