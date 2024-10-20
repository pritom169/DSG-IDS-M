# Chapter-03
## Process System Model
In distributed systems, the **Process System model** describes how processes
interact and communicate with each other. Here are some key aspects:

1. **Processes**: Independent entities that execute concurrently. Each 
process has its own local memory and state.
2. **Communication**: Processes communicate by passing messages since 
they do not share a global memory. This communication can be asynchronous, 
meaning message delivery times are unpredictable.
3. **Event Ordering**: Events within a process are linearly ordered, but 
across processes, events are ordered using logical clocks to maintain 
causal relationships.
4. **Failure Models**: These describe how processes and communication can 
fail. Common types include omission failures (e.g., message loss) and 
Byzantine failures (e.g., arbitrary behavior).

### Static Structure
- Defines the fixed, unchanging aspects of processes.
- Describes how processes are organized
- Represents system architecture

### Dynamic Behavior
* Describes how processes interact over time
* Shows runtime activities
* Represents actual system operations

### Example:
**Static Structure:**

ATM System:
- ATM Terminal (Process A)
- Bank Server (Process B)
- Database (Process C)

**Dynamic Structure:**

User withdraws money:
* A → B: Send withdrawal request
* B → C: Check balance
* C → B: Balance confirmed
* B → A: Approve withdrawal
* A: Dispense cash

If system fails:
- A: Display error
- B: Rollback transaction
- C: Maintain consistency

## Sequential and Parallel Execution
### Sequential Execution
* Tasks are executed one after another in a specific order
* Each task must complete before the next one begins

### Parallel Execution
* Multiple tasks are executed simultaneously
* Tasks run independently on different nodes or processors

## Deterministic System and Determinate System
### Deterministic System
Definition: A system where the same input always produces the same output 
when executed under identical conditions

Examples:
1. A simple calculator performing arithmetic operations
2. A sorting algorithm with a fixed sorting method

### Determinate System
Definition: A system where the output is predictable regardless of 
execution speeds or timing of different processes

Examples:
1. A distributed banking system maintaining consistent balances
2. A message passing system with ordered delivery

## Blocking, Deadlocks and Starvation
1. Blocking:
- Temporary delay when process waits for resource
- Normal part of system operation
- Resolves when resource becomes available

2. Deadlock:
- Processes permanently blocked waiting for each other
- Requires four conditions:
    * Mutual exclusion
    * Hold and wait
    * No preemption
    * Circular wait
- System comes to standstill

3. Starvation:
- Process never gets resources despite being eligible
- Often caused by unfair resource allocation
- Process stays alive but makes no progress

Connection:
- Blocking can lead to deadlock if circular dependencies form
- Deadlock resolution might cause starvation of some processes
- Poor handling of blocking can result in starvation

## Deadlock detection
Main Deadlock Detection Methods:

1. Wait-For Graph:
- Records which process is waiting for other processes
- Deadlock exists if there's a cycle in the graph

2. Resource Allocation Graph:
- Shows resources held and requested by processes
- Deadlock exists if cycle meets specific conditions

3. Timeout-Based Detection:
- Set timeouts for resource requests
- If timeout occurs, assume deadlock

4. Centralized Detection:
- Single detector maintains global state
- Periodically checks for deadlocks

5. Distributed Detection:
- Multiple detectors work together
- Exchange state information

> Remembering Technique: WRTCD

## Asynchronous vs Synchronous Communication System
Key Differences:

1. Timing:
- Synchronous:
  * Sender waits for response
  * Fixed timing and order
  * Blocking operations
- Asynchronous:
  * Sender continues execution
  * Flexible timing
  * Non-blocking operations

2. Performance:
- Synchronous:
  * Slower overall throughput
  * Resource underutilization
  * Predictable performance
- Asynchronous:
  * Better throughput
  * Better resource utilization
  * Variable performance

3. Implementation:
- Synchronous:
  * Simpler to implement
  * Easier to debug
  * Sequential flow
- Asynchronous:
  * More complex
  * Harder to debug
  * Parallel flow

4. Use Cases:
- Synchronous:
  * Email is an example of asynchronous communication. You send an email, 
  and the recipient reads it at their convenience, not necessarily 
  immediately.
- Asynchronous:
  * A phone call is an example of synchronous communication. You speak 
  and listen in real-time, requiring immediate interaction.

## Shared Memory Systems and Distributed Memory Systems
### Shared Memory Systems
- Common memory space.
- Synchronization with locks and shared variables.
- Communication through shared memory.

### Synchronization in SMS
- Ensure that only one process accesses a shared resource at a time
- Define rules for the order in which memory operations are visible to
  processes to maintain data consistency.
- Use techniques like deadlock detection and prevention to avoid
  processes getting stuck.

### Distributed Memory Systems
- Separate memory spaces.
- Synchronization via message-passing.
- Communication through messages.

### Synchronization in DMS
> Memory Space: COCA rule

C - Correct Order
- Keep messages in sequence
- Like sorting emails by time
- Prevents message mix-ups

O - Organized Access
- Take turns using resources
- Like sharing a printer
- Prevents conflicts

C - Consensus Agreement
- Group decision making
- Like team voting
- Handles failures gracefully

A - Accurate Time
- Keep all clocks in sync
- Like coordinating watches
- Ensures timing accuracy

## Peterson's Algorithm
Peterson's algorithm is a way for two processes (or threads) to take 
turns using a shared resource without interfering with each other. 
It's a solution to the "critical section problem," where multiple 
processes need to access shared data, but only one should do so at a 
time to avoid conflicts.

### How it works:

1. **Process A wants to enter the critical section**:
  - It sets `flag[0] = true` to indicate that it wants to enter.
  - It sets `turn = 1`, meaning it's giving priority to Process B.

2. **Process B wants to enter the critical section** at the same time:
  - It sets `flag[1] = true` and `turn = 0` (giving priority to Process A).

3. **Check who goes in**:
  - Process A can only enter if either Process B doesn’t want to enter 
(`flag[1] = false`) or it's Process A’s turn (`turn = 0`).
  - Similarly, Process B can enter if Process A doesn't want to enter 
(`flag[0] = false`) or it's Process B’s turn (`turn = 1`).

4. **Take turns**: This setup ensures that the processes alternate access 
if they both want to enter the critical section at the same time, avoiding 
conflicts.

5. **Exit critical section**: Once a process is done in the critical 
section, it sets its flag to `false`, letting the other process have a 
turn if needed.

### Key points:
- It ensures **mutual exclusion** (only one process can be in the 
critical section at a time).
- It avoids **deadlock** (both processes waiting forever).
- It gives **progress** (each process will eventually get a chance).


## De-Coupling using Selective Receives
### What is Decoupling?
Decoupling refers to the idea of separating different parts of a system 
so that changes or interactions in one part don’t heavily impact another.

### What is Selective Receives?
Selective Receives is a feature in some message-passing systems 
that allows a process to pick and choose which messages 
it wants to handle from its mailbox, based on certain conditions.

### How Selective Receives Enable Decoupling
In a typical system:

- The sending process doesn’t 
wait for the receiver to handle the message. It sends and moves on.
- The receiving process collects messages in its mailbox, and with selective 
receive, it can prioritize which messages it handles based on its current 
state or need.

This setup helps decouple:

- The sender from the receiver (the sender doesn't have to know when or how 
the receiver will handle the message).
- The receiver’s logic from the message order (the receiver can handle 
high-priority messages first).

## Message Passing Synchronization
1. **One-way Synchronization:** A process sends a signal and continues its 
work while expecting a response from another process. 
2. **Mutual Exclusion for Shared Data:** Ensures that only one process can 
write to shared data at a time. 

Explanation:

- In one-way synchronization, a process initiates an action without 
waiting for an immediate response. This is like sending a message and 
expecting a reply later.
- Mutual exclusion for shared data ensures that access to critical data 
is controlled to prevent concurrent writes, which could lead to data 
corruption or inconsistencies.

## How to overcome unreliable transport in Distributed System
1. **Use Acknowledgments and Retransmissions:** Senders wait for
acknowledgements from receivers and retransmit if needed.
2. **Implement Sequence Numbers:** Assign unique sequence numbers to messages to maintain
   order.
3. **Utilize Checksums and Error Detection:** Include checksums to detect message corruption.
4. **Set Timeouts and Heartbeats:** Use timeouts to monitor remote processes or nodes.
5. **Detect and Deduplicate Duplicates:** Track received message IDs to avoid duplicates.
6. **Apply Flow Control:** Prevent sender overload to avoid congestion.