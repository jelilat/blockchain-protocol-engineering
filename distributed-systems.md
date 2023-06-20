# Distributed Systems

A distributed system is a network of computers that work together as a single system to solve a computing problem. In a distributed system, each computer in the network is called a **node**, and these nodes communicate with each other by passing messages over a network.

<figure><img src="https://lh4.googleusercontent.com/VUoYMmCuINiN1Ycs-NSJ_y4r9hC6AODjTzZzd6_rkuQRP--lo4WzKdcno6j3lQkcEqM2FonIJ3UJAFKR4blDp_h3SsSiHll0178Gj7QnMI2AGffdCKZ8xxfkSKz0OQG02Jo2-YgSL-Duko32EoqvWzrANArxLldP=s2048" alt="" width="375"><figcaption></figcaption></figure>

Let's consider a case whereby we are required to solve 6 differential equation problems in a given timeframe. The work, in this case, is these six differential equations. If we were to tackle this problem using a single computer system, it would mean this single node has to solve each equation one by one, which could be time-consuming.

However, in a distributed system, this workload can be divided among six nodes, each assigned to solve one differential equation. This is known as the principle of workload distribution or task division in distributed computing. It allows for tasks to be broken down into smaller subtasks, each capable of being solved concurrently by different nodes in the system, resulting in a considerable reduction in total processing time.

Let's assume that each node in our distributed system is capable of solving one differential equation in 3 seconds. If we had only one node (as in a single computer system), it would take 18 seconds to complete all six problems. But, in our distributed system, where each of the six nodes is working on one problem simultaneously, all six differential equations could be solved in just 3 seconds.

Moreover, in Peer-to-Peer (P2P) distributed systems, the results from each node can be shared directly with the other nodes. Instead of relying on a central server or authority for aggregation, each node in the network contributes to the final output by processing its part of the task and sharing the results with its peers.

Also, a distributed system is designed to continue functioning even if one or several nodes fail, by redistributing the tasks among the remaining functional nodes. This concept is known as fault tolerance. If a node assigned to solve a differential equation fails in our case, its task can be reassigned to another operational node to ensure continuous progress.

Therefore, by employing a distributed system, we can significantly enhance the efficiency, speed, and resilience of the process of solving multiple complex tasks such as differential equations.

<img src="https://lh5.googleusercontent.com/6hp9iDufeq3ZnnoSW3IDBELOGXfB2Lp3D8JA_A9DBrBTDv8ZJLvM1-1jObn6g4-jSNXHBoa3gJwupLJkaJeyIhXCyDJ39xlInZceD5BZEESCQaKEJrgbvjdjBOdb6MSFiCeU8E0B2dDb1sPp8dSoT4DbNXQCpC8E=s2048" alt="" data-size="original">                              ![](https://lh3.googleusercontent.com/xevXIopHnOTGB2unMHH7NuzLd\_dL3sZrMkiOjg8W-LYQrmT27\_6rgGarKSyD6CYVwxQjBMeP5PD5gDmGWy1EhNclAqxAqR4QXR8pRBop9ExAGhfUjf6XO3DUhYr-iB5mTCANxbiFw4wugS6iVev0NTyzRmrGWwix=s2048)
