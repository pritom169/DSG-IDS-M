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

### Synchronization
- Ensure that only one process accesses a shared resource at a time
- Define rules for the order in which memory operations are visible to
  processes to maintain data consistency.
- Implement mechanisms to manage concurrent access to shared data
  structures without conflicts
- Use techniques like deadlock detection and prevention to avoid
  processes getting stuck.

### Shared Memory Systems
- Separate memory spaces.
- Synchronization via message-passing.
- Communication through messages.

### Synchronization
- Maintain the correct order of messages to ensure consistency and
  prevent race conditions.
- Coordinate access to shared resources across distributed nodes using
  distributed lock managers.
- Achieve agreement among distributed nodes even in the presence of failures
  using consensus algorithms.
- Ensure synchronized clocks across nodes for accurate timing and
  coordination.


