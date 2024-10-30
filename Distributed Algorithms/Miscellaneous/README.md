## Two-Phase Commit(2PC)

The **Two-Phase Commit (2PC)** protocol is a method used in distributed systems to ensure all participants in a transaction agree to commit (finalize) or abort (cancel) the transaction, guaranteeing data consistency across all nodes.

### Overview of 2PC

In a 2PC protocol, there is a **coordinator** and multiple **participants** (nodes or systems) involved in a transaction. The protocol consists of two main phases:

1. **Phase 1: Prepare (Voting Phase)**

   - The **coordinator** sends a "prepare" message to all participants, asking if they are ready to commit.
   - Each **participant** checks if it can complete the transaction (e.g., it has necessary resources, no errors) and responds:
     - **"Yes"** (Ready to commit) if it’s able to commit the transaction.
     - **"No"** (Abort) if it encounters an issue.

2. **Phase 2: Commit or Abort (Decision Phase)**
   - If the coordinator receives "Yes" votes from all participants, it sends a **"commit"** message to finalize the transaction.
   - If any participant replies with a "No," the coordinator sends an **"abort"** message to all participants, canceling the transaction.
   - Each participant then either commits or aborts, following the coordinator's decision.

### Example of Two-Phase Commit

Suppose a bank transaction involves transferring money from **Account A** (in System X) to **Account B** (in System Y):

1. **Prepare Phase**:

   - The coordinator requests **System X** and **System Y** to check if they can complete the transaction.
   - Both systems check if they can lock the necessary resources (e.g., sufficient funds in Account A) and reply **"Yes"** or **"No"**.

2. **Commit/Abort Phase**:
   - If both systems reply "Yes," the coordinator sends a "commit" command to complete the transfer.
   - If any system replies "No" (e.g., insufficient funds), the coordinator sends an "abort" command, and the transaction is canceled.

### Three Phase Commit

The **Three-Phase Commit (3PC)** protocol is an enhancement of the Two-Phase Commit (2PC) protocol used to achieve consensus in distributed systems, with improvements to prevent blocking if the coordinator fails.

### Phases of the Three-Phase Commit Protocol

1. **Phase 1: CanCommit (Prepare/Pre-Vote Phase)**

   - The **coordinator** sends a "can commit?" message to all participants, asking if they’re prepared to commit the transaction.
   - Each **participant** responds with "Yes" (prepared to commit) or "No" (unable to commit).

2. **Phase 2: PreCommit (Prepared/Waiting Phase)**

   - If the coordinator receives unanimous "Yes" responses, it sends a **"pre-commit"** message to all participants, signaling them to prepare for the commit.
     - Each participant performs necessary preparations (e.g., locks resources) and sends back an acknowledgment ("ack") that they’re ready.
   - If any participant responded with "No" in Phase 1, the coordinator sends an **"abort"** message to cancel the transaction.

3. **Phase 3: Commit or Abort (Decision Phase)**
   - After receiving acknowledgments from all participants in the PreCommit phase, the coordinator sends a **"commit"** message to finalize the transaction.
   - If any participant fails to acknowledge, the coordinator sends an **"abort"** message to all participants.
   - Each participant then either commits or aborts, following the coordinator’s decision.

### Example of Three-Phase Commit

Consider a transaction involving two systems, **System X** and **System Y**, where money is to be transferred from Account A in System X to Account B in System Y.

1. **CanCommit Phase**:

   - The coordinator asks System X and System Y, "Can you commit this transaction?"
   - Each system checks for the necessary conditions (e.g., funds availability) and replies "Yes" (able to commit) or "No" (unable to commit).

2. **PreCommit Phase**:

   - If both systems reply "Yes," the coordinator sends a "pre-commit" message, indicating they should prepare to commit.
   - Each system locks resources (e.g., reserves funds in Account A) and sends back an acknowledgment that it’s ready.

3. **Commit/Abort Phase**:
   - If the coordinator receives all acknowledgments, it sends a "commit" message, finalizing the transfer.
   - If any system fails to respond, the coordinator sends an "abort" message to cancel the transaction.

### Key Advantages of 3PC

- **Non-blocking**: Unlike 2PC, 3PC avoids indefinite waiting by introducing the PreCommit phase, which ensures participants can still make decisions even if the coordinator fails.
- **Fault Tolerance**: If participants or the coordinator fail, 3PC allows remaining nodes to reach a consensus based on the state (pre-commit or abort) they’re in.

3PC is especially useful for distributed systems where minimizing blocking is critical and ensuring a robust fail-safe mechanism is required. However, 3PC is complex and generally more resource-intensive than 2PC, so it's used selectively in systems requiring high reliability.

## Why do many DS use 2PC, when 3PC is better.

Two-Phase Commit (2PC) is still commonly used in distributed systems because of
its simplicity, legacy compatibility, and speed of exectution.

## CAP Theorem

The **CAP Theorem** (Consistency, Availability, Partition Tolerance) states that in a distributed data system, it is impossible to simultaneously achieve all three properties. A system can achieve at most two of these three:

1. **Consistency (C)**: Every read receives the latest write. All nodes see the same data at the same time.
2. **Availability (A)**: Every request (read/write) receives a response, even if some nodes are down.
3. **Partition Tolerance (P)**: The system continues to operate despite network partitions (communication failures between nodes).

### Trade-Offs in CAP

Since no distributed system can guarantee all three simultaneously, systems must choose which two properties to prioritize:

- **CP (Consistency and Partition Tolerance)**: The system ensures consistency but may sacrifice availability during network issues (e.g., traditional databases like RDBMS).
- **AP (Availability and Partition Tolerance)**: The system remains available but may show outdated data due to inconsistency (e.g., some NoSQL databases like Cassandra).
- **CA (Consistency and Availability)**: Can only be fully achieved in single-node systems without network partitioning, so it's generally impractical in distributed systems.

### Pros and Cons of the CAP Theorem

#### Pros

- **Framework for Design**: Provides a clear model for designing distributed systems by understanding trade-offs.
- **System Specialization**: Enables tailored solutions based on application needs (e.g., real-time analytics vs. transaction processing).

#### Cons

- **Binary Limitation**: Critics argue that the CAP theorem oversimplifies the complexity
  of real-world distributed systems. It doesn't account for nuances like eventual
  consistency or the specific implementations and optimizations used by various
  distributed databases.

- **Not Complete**: Ignores other important aspects like latency, scalability, and data durability, which are also crucial in distributed systems.
