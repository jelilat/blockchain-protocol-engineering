# Distributed Systems

A distributed system is a network of computers that work together as a single system to solve a computing problem. In a distributed system, each computer in the network is called a **node**, and these nodes communicate with each other by passing messages over a network.

<figure><img src="https://lh4.googleusercontent.com/VUoYMmCuINiN1Ycs-NSJ_y4r9hC6AODjTzZzd6_rkuQRP--lo4WzKdcno6j3lQkcEqM2FonIJ3UJAFKR4blDp_h3SsSiHll0178Gj7QnMI2AGffdCKZ8xxfkSKz0OQG02Jo2-YgSL-Duko32EoqvWzrANArxLldP=s2048" alt="" width="375"><figcaption></figcaption></figure>

Let's consider a case where we need to solve six differential equations in a given timeframe. Here, our task is to solve these six equations. If we were to tackle this using a single computer, that lone node would handle each equation one by one, which could be time-consuming.

However, in a distributed system, this workload can be divided among six nodes, each assigned to solve one equation. This is known as the principle of workload distribution or task division in distributed computing. It allows tasks to be broken down into smaller subtasks that can be solved concurrently, resulting in a significant reduction in total processing time.

Imagine it like a restaurant with six chefs, each responsible for one dish. Rather than one chef cooking everything sequentially, each chef handles a dish simultaneously. The entire meal is ready much faster since the tasks are divided among the chefs, just like in our system where each node handles one equation.

Let us assume that each chef can cook a meal in three minutes. With only one chef, it would take 18 minutes to complete all six dishes. But, with a distributed system where each of the six chefs prepares a meal, all 6 dishes can be prepared in just 3 minutes.

In Peer-to-Peer (P2P) distributed systems, each node shares results directly with its peers, much like chefs in a kitchen who coordinate directly with each other rather than a central chef to ensure everything is ready together. Each node contributes to the final output without needing a central authority to assemble the results.

And if one chef or node fails to complete their task? No worries. In distributed systems, fault tolerance enables the remaining nodes to pick up the work, ensuring continuous progress. In our case, if a node assigned to solve an equation fails, its task can be reassigned to another operational node, ensuring we stay on track.

Therefore, by employing a distributed system, we enhance efficiency, speed, and resilience—whether we’re solving complex tasks or preparing a multi-course meal!

<img src="https://lh5.googleusercontent.com/6hp9iDufeq3ZnnoSW3IDBELOGXfB2Lp3D8JA_A9DBrBTDv8ZJLvM1-1jObn6g4-jSNXHBoa3gJwupLJkaJeyIhXCyDJ39xlInZceD5BZEESCQaKEJrgbvjdjBOdb6MSFiCeU8E0B2dDb1sPp8dSoT4DbNXQCpC8E=s2048" alt="" data-size="original">                              ![](https://lh3.googleusercontent.com/xevXIopHnOTGB2unMHH7NuzLd\_dL3sZrMkiOjg8W-LYQrmT27\_6rgGarKSyD6CYVwxQjBMeP5PD5gDmGWy1EhNclAqxAqR4QXR8pRBop9ExAGhfUjf6XO3DUhYr-iB5mTCANxbiFw4wugS6iVev0NTyzRmrGWwix=s2048)
