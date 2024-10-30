# Distributed Deadlock Detection

## Distributed Deadlock

Distributed deadlock occurs in distributed systems when multiple processes are
waiting for resources held by each other, resulting in a cycle of dependencies and
causing all processes to be blocked. Detection and resolution mechanisms are needed
to manage distributed deadlocks and allow processes to continue execution.

## The Distributed Edge Chasing Algorithm

The Distributed Edge Chasing Algorithm is a termination detection algorithm
used in distributed systems. Specifically, it detects whether there exists any cycle in a
distributed system, which might indicate a deadlock.

### Working Mechanism

1. **Initiation**: When a process \( P_i \) is waiting for a resource held by another process \( P_j \), it initiates a probe. This probe contains information identifying the initiator \( P_i \) and the current waiting process \( P_j \).

2. **Propagation**: If \( P_j \) is also waiting for a resource held by another process \( P_k \), it forwards the probe to \( P_k \). The probe continues to be forwarded through the chain of processes that are waiting for each other.

3. **Detection**: If the probe returns to the initiator \( P_i \), a cycle is detected, indicating a deadlock involving \( P_i \) and potentially other processes along the cycle.

4. **Resolution**: Upon detecting a deadlock, the system can then apply a deadlock resolution strategy, such as terminating a process or preempting resources to break the cycle.

### Example

Consider three processes \( P_1 \), \( P_2 \), and \( P_3 \) in a distributed system where:

- \( P_1 \) waits for a resource held by \( P_2 \),
- \( P_2 \) waits for a resource held by \( P_3 \),
- \( P_3 \) waits for a resource held by \( P_1 \).

1. **Probe Initiation**: \( P_1 \) initiates a probe indicating that it is waiting for \( P_2 \).
2. **Probe Propagation**: \( P_2 \) forwards the probe to \( P_3 \), adding its own dependency information.
3. **Cycle Detection**: When \( P_3 \) forwards the probe back to \( P_1 \), \( P_1 \) identifies that the probe has returned to it, signifying a cycle.

The cycle \( P_1 \rightarrow P_2 \rightarrow P_3 \rightarrow P_1 \) indicates a deadlock.
