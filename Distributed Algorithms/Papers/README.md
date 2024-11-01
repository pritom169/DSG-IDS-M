# Papers

## 1 The Byzantine Generals Problem

Two conditions for Byzantine Generals Problem.

### Introduction

This section introduces the problem of achieving consistency in a distributed system, particularly when certain components may behave inconsistently or maliciously. The authors use the Byzantine Generals Problem as an analogy, where generals must agree on a strategy but may face traitors trying to disrupt consensus.

### A Solution with Oral Messages

In this section, the authors introduce the OM(m) algorithm, which can achieve consensus among generals if more than two-thirds are loyal. This algorithm works by repeatedly passing messages between generals, where each general receives, verifies, and passes along information from others. The authors rigorously prove the conditions under which this solution works, specifically that it requires at least 3m+1 generals to handle m traitors.

The authors provided two key proofs in _The Byzantine Generals Problem_: the proof that \(3m + 1\) generals are needed to handle \(m\) traitors with oral (unsigned) messages, and the concept of unforgeable signed messages that allow for consensus with any number of generals.

#### Proof of the \(3m + 1\) Requirement

The authors proved that achieving consensus with only oral messages (messages without authentication) requires at least \(3m + 1\) generals to tolerate \(m\) traitors.

1. **Base Case with 3 Generals**:

   - If there are only 3 generals, a single traitor can prevent the remaining loyal generals from agreeing. For instance, if the commander is loyal and issues an "attack" order, a traitorous general can report conflicting information ("retreat") to different loyal generals.
   - The loyal generals then receive conflicting orders and cannot determine which one to trust, leading to a failure in consensus.

2. **Inductive Step**:
   - Using the results from the 3-general case, the authors extend their reasoning to larger groups. They show that if a system with \(3m\) generals could handle \(m\) traitors, then a smaller group with only three generals could also reach consensus with a single traitor (which has been proven impossible).
   - By contradiction, this implies that a solution for \(3m + 1\) or more generals is necessary to handle \(m\) traitors.

### A Solution with Signed Messages

Here, the authors describe a solution that uses signed, unforgeable messages, which allow loyal generals to verify the source of each command. This solution is more resilient since it works with any number of generals, provided each message's authenticity can be verified. The SM(m) algorithm is introduced, enabling consensus even if some generals are traitors, as long as loyal generals can distinguish real messages from those altered or forged by traitors.

#### Description of Unforgeable Messages

The unforgeable message concept, which enables consensus with any number of generals, is defined by assumptions on message signatures that prevent traitors from mimicking or altering authentic messages from loyal generals. Here’s how the paper describes and enforces the unforgeability:

1. **Assumptions of Signed Messages**:

   - **A4(a)**: A loyal general’s signature cannot be forged, and any alteration of the message’s contents can be detected.
   - **A4(b)**: Anyone can verify the authenticity of a general’s signature, meaning all generals can validate whether a message truly came from a specific general without alteration.

2. **Signature Function (Si)**:

   - The signature function \(S_i(M)\) produces a unique signature for a message \(M\) from general \(i\). For a message to be authentic, the signature \(S_i(M)\) must match the data item \(M\), which is validated by each recipient.
   - The authors propose using cryptographic techniques to make signatures infeasible to forge. For example, they suggest that signatures be computed with a randomized function or a cryptographic method that is computationally secure against forgery attempts.

3. **Approaches for Different Failure Types**:
   - **Random Malfunctions**: If the issue is random component failure, the authors propose a randomized function that makes the probability of successful forgery negligibly low.
   - **Malicious Interference**: For cases where a traitor might be attempting to forge signatures deliberately (as in an attack by a malicious party), cryptographic methods, such as public-key signatures (as later formalized in systems like RSA), can ensure that any tampering with a message’s content or origin is detectable.

#### Missing Communication Paths

In the _Missing Communication Paths_ section, the authors address situations where generals (or processors in a distributed system) do not have direct communication with each other. In these cases, not all generals can exchange messages directly, which introduces additional challenges to maintaining consensus. They adapt their algorithms (OM and SM) for partially connected networks by introducing a communication graph with specific connectivity properties.

### The OM(m, p) Algorithm for Partially Connected Networks

The authors extend the OM algorithm to work with these partially connected graphs by specifying a process in which generals relay messages along verified paths in the communication graph. Here’s a breakdown of OM(m, p):

1. **Choose Regular Set of Neighbors**:

   - The commander (originator) selects a regular set of \( p \) neighboring generals, which satisfies the connectivity requirements. These neighbors will act as intermediaries to relay messages to the rest of the generals.

2. **First Round of Communication**:

   - The commander sends the command to all neighbors in this regular set. Each neighbor receives the commander’s message, either directly or, if necessary, via reliable paths that do not pass through traitors.

3. **Relay Using Sub-Algebra (OM(m - 1, p - 1))**:

   - Each general in the regular set relays the command to the other generals in the network. This process involves applying a nested version of the algorithm, OM(m - 1, p - 1), using subgraphs formed by excluding the commander (as well as any previously visited nodes).
   - The algorithm works recursively, where each level down the recursion tree removes one layer of generals until all generals receive the message.

4. **Final Decision**:
   - After all messages have been relayed through multiple levels, each general computes a majority vote based on the messages received from their regular set of neighbors. The majority value across these redundant paths determines the final consensus.

##### Ensuring Consensus and Fault Tolerance

The authors prove that OM(m, p) can tolerate up to \( m \) traitors if the graph is \(3m\)-regular, meaning each general has at least \(3m\) reliable neighbors. This connectivity ensures:

- **Consistency**: Even if a traitor disrupts some paths, the redundancy allows loyal generals to receive consistent information from other neighbors.
- **Agreement**: The recursion and message-passing structure guarantee that all loyal generals have a consistent set of messages from which they can compute the same majority value.

## 2 Transactions in a Microservice World

### ACID Properties

In distributed systems, **ACID** properties ensure reliable transactions:

1. **Atomicity**: Transactions are all-or-nothing; if one part fails, the whole transaction rolls back.
2. **Consistency**: Transactions bring the system from one valid state to another, maintaining data integrity.
3. **Isolation**: Concurrent transactions don’t interfere with each other.
4. **Durability**: Once committed, transactions persist despite crashes.

### Distributed Transactions

- **Multiple Storage Systems:** Distributed transactions become necessary when businesses use varied storage systems (e.g., relational, graph databases, or message queues).

- **Two-Phase Commit Protocol(2PC):** The 2PC protocol ensures that all systems commit to a transaction or roll it back simultaneously, divided into a "voting" phase where systems prepare to commit, and a "completion" phase where the transaction is either finalized or rolled back. A transaction manager coordinates this process, ensuring that atomicity is maintained.

- **Data Replication:** Distributed systems often replicate data across different nodes for reliability.

- **Implicit Use of Replication:** In distributed databases, replication is usually handled by middleware. While transparent to users, replication affects application design and consistency, especially in cloud applications where data replication and network delays can disrupt synchronization.

### Highly Distributed Transactions

- **CAP Theorem:** In distributed systems, the CAP theorem posits that achieving all three—Consistency, Availability, and Partition-tolerance—is impossible, forcing applications to prioritize between availability and consistency when partitions occur.

- **Consistency Models:** In contrast to strict ACID consistency, distributed applications often use looser models:
  - **Strict Consistency** ensures that all replicas instantly reflect changes, which is challenging in distributed systems.
  - **Eventual Consistency** guarantees that all replicas will synchronize over time but allows temporary inconsistencies.
- **BASE Paradiagram:**

  1. **Basically Available**: The system is mostly available, even if parts of it fail, meaning users can access data but may experience delays or partial results.

  2. **Soft State**: The system state may change over time due to the eventual consistency model, allowing temporary inconsistencies during processing.

  3. **Eventual Consistency**: Data will eventually reach a consistent state across all nodes, but there is no guarantee of immediate consistency after each transaction.

- **Concurrency Control:** To manage concurrent access across multiple microservice instances, applications use methods like pessimistic locking (prevents access during operations) and optimistic locking (checks for conflicts without locks).

### Compensation-Based Transactions:

- **Principle Behind Rollback:** Traditional rollback methods undo failed transactions by restoring previous data states, typically using logs to revert changes.
- **Dawn of Compensation:** For long-running transactions, operations are divided into smaller transactions or "mini-batches" that can individually complete and release locks. If a mini-batch fails, compensatory logic, or "chained" transactions, can be applied to reverse changes.
- **Semantic Recovery:** Compensation-based transactions rely on application-specific compensatory actions (e.g., issuing refunds for failed transactions) instead of strict data restoration. This approach, termed semantic recovery, requires programmers to write specific compensatory actions for various transaction steps, providing flexibility for cloud applications.
- **Compensation Spheres and SAGAs:**

  - The **SAGA** model is a design pattern for managing long-running transactions in distributed systems by breaking them down into a series of smaller, isolated sub-transactions. Each sub-transaction has a **compensating action** to undo its effect if something fails later in the chain. SAGA ensures data consistency without locking resources, ideal for systems prioritizing high availability.

  1. **Chained Sub-transactions**: The main transaction is divided into steps, each independently successful.
  2. **Compensating Actions**: Each step has a reverse action, allowing rollback of prior actions if a failure occurs.
  3. **Eventual Consistency**: SAGA ensures eventual consistency without needing a single, all-or-nothing transaction, supporting scalability.

## 3 Consistency Tradeoffs in Modern Distributed Database System Design

### 1. Introduction

- This section explains the background of distributed database systems, especially the increasing industrial use of DDBSs due to modern demands for scalability and data proximity. It introduces various DDBSs like Dynamo, Cassandra, PNUTS, and Riak.
- The CAP theorem is introduced, noting its limitations, particularly the focus on failure conditions. The author argues that consistency/latency trade-offs are more influential in DDBS design than the CAP theorem alone.

### 2. CAP is for Failures

- **Summary of CAP’s Scope**:
  - In essence, the CAP theorem is primarily relevant in the event of network failures and does not dictate everyday operational constraints. Systems can operate with full consistency and availability outside of partition events, allowing designers to balance performance and reliability based on real-world scenarios rather than CAP restrictions alone.

### 3. "Consistency/Latency Tradeoff"

- **Summary of Consistency/Latency Dynamics**:
  - This section concludes that ignoring the consistency/latency trade-off in favor of CAP alone oversimplifies DDBS design, as latency considerations are critical to daily system operation.

### 4. **Data Replication**:

- Detailed options for data replication are discussed, emphasizing consistency and latency trade-offs based on the chosen replication strategy.
- The strategies include synchronous updates across all replicas, updates sent to a designated master node, and arbitrary node-first updates. Each method affects latency and consistency differently, depending on the order and location of updates.

### 5. **Tradeoff Examples**:

- The section provides examples of various DDBSs, including Dynamo, Cassandra, PNUTS, and Riak, each illustrating different consistency and latency trade-off strategies.
- It compares their approaches to replication and consistency, using real-world studies to demonstrate the impact of these trade-offs on system performance and user experience.

### 6. **PACELC**:

- The PACELC model is introduced as an extension of the CAP theorem to include latency considerations. PACELC adds an "Else Consistency" (ELC) clause, covering the trade-off between latency and consistency when no network partition exists.
- Different DDBSs are classified under PACELC, showing how each system balances availability, consistency, and latency in normal and partitioned states.

### 7. **Conclusion**:

- The article concludes by underscoring that both CAP and PACELC highlight important trade-offs in DDBS design. However, PACELC offers a more nuanced understanding of the continuous balance between consistency and latency.
- The author advocates for broader recognition of the consistency/latency trade-off, as it directly impacts system operation more frequently than network partitions alone.

## 4. CAP Twelve Years Later: How the Rules Have Changed

### Introduction to CAP Theorem

The CAP theorem posits that a networked shared-data system cannot achieve more than two out of three properties: **Consistency (C)**, **Availability (A)**, and **Partition Tolerance (P)**. Initially introduced to broaden design possibilities, it highlighted trade-offs, especially relevant for distributed systems in the NoSQL era. The paper argues that "2 of 3" is a simplification and that real-world scenarios offer more nuanced trade-offs.

### The "2 of 3" Misinterpretation

CAP often leads to the misconception that designers must always choose between C and A in partition scenarios. However, this oversimplifies the actual dynamics. For instance, system components can prioritize either consistency or availability depending on current conditions, specific operations, or data involved, adding flexibility. This re-framing allows for dynamically balancing CAP properties rather than a rigid binary choice.

### CAP-Latency Connection

Latency is critical in CAP's practical application. Since timeouts drive the partition decision (whether to continue an operation, risking inconsistency, or to cancel it, compromising availability), the design can adapt by setting response-time goals. Systems with strict latency goals may enter partition mode even with minor network slowdowns, impacting perceived availability and consistency.

### Managing Partitions

Designers need explicit partition management, involving three steps:

1. **Detect partitions** – Recognize the beginning of a network partition.
2. **Enter partition mode** – Limit operations or capture essential information for recovery.
3. **Recover from partition** – Upon re-establishing communication, restore consistency and compensate for any partition-induced errors.

This strategy minimizes CAP trade-offs while maintaining operational continuity.

### ACID, BASE, and CAP

The **ACID** model focuses on consistency, contrasting with **BASE** (Basically Available, Soft state, Eventually consistent) which leans towards availability and eventual consistency. CAP properties interact uniquely with ACID, primarily by isolating transactions to maintain consistency at the cost of availability during partitions. Conversely, BASE tolerates network partitions, leading to designs favoring availability and recovery from inconsistency post-partition.

### Partition Recovery

Upon resolving a partition, two tasks remain: synchronizing states across nodes and addressing any invariant violations caused during the partition. Using techniques like version vectors or conflict-free replicated data types (CRDTs) enables system states to converge post-partition while tracking and compensating for partition-induced issues.

### Compensation for Inconsistencies

Partitions can lead to errors like duplicate or lost operations. To address this, systems use compensating transactions, which correct errors by adjusting affected data. Real-world examples, like ATMs, may limit withdrawals during a partition to avoid overdraft errors, which can then be corrected through compensatory actions when network stability is restored.

### Practical Applications and Conclusion

The paper concludes by emphasizing the importance of balancing CAP properties pragmatically, acknowledging that emerging data structures and synchronization techniques allow greater flexibility. As systems grow more sophisticated, nuanced CAP management, involving custom trade-offs, can be more effective than rigid adherence to the original "2 of 3" rule.

This document emphasizes a more nuanced, situational approach to the CAP theorem, urging designers to handle partitions actively and manage recovery to balance consistency and availability effectively.
