## Casualty Violation
Causality in distributed systems is based on the happens-before (→) 
relationship, introduced by Lamport. If one event causes or influences 
another, the first event must occur before the second. For example, if 
process P1 sends a message to process P2, the sending of the message must 
happen before the receiving of the message.

## Lamport's Logical Clock
Lamport's Logical Clock is a simple but powerful algorithm for creating a 
partial ordering of events in a distributed system.

1. Each process starts with a timestamp of 0.
2. When an event happens, the process increases its timestamp by 1.
3. Messages include the sender's timestamp.
4. When a process receives a message, it updates its timestamp to be 
greater than or equal to the sender's timestamp.
5. Events are ordered based on their timestamps, helping track causality 
and ensuring consistent event order in distributed systems.

**Example:**

1. Initially, all clocks are set to 0:
    - \( P1 = 0 \)
    - \( P2 = 0 \)
    - \( P3 = 0 \)

2. Event \( E1 \) happens at \( P1 \), so \( P1 \) increments its clock:
    - \( P1 = 1 \) (after \( E1 \))
    - \( P2 = 0 \)
    - \( P3 = 0 \)

3. \( P1 \) sends a message to \( P2 \), so it increments its clock and sends the message with the timestamp \( 2 \):
    - \( P1 = 2 \) (after sending)
    - \( P2 = 0 \)
    - \( P3 = 0 \)

4. \( P2 \) receives the message. \( P2 \)’s clock is updated to \( \text{max}(0, 2) + 1 = 3 \):
    - \( P1 = 2 \)
    - \( P2 = 3 \)
    - \( P3 = 0 \)

5. Event \( E2 \) happens at \( P2 \), so \( P2 \) increments its clock:
    - \( P1 = 2 \)
    - \( P2 = 4 \) (after \( E2 \))
    - \( P3 = 0 \)

6. \( P2 \) sends a message to \( P3 \), increments its clock, and sends the message with the timestamp \( 5 \):
    - \( P1 = 2 \)
    - \( P2 = 5 \) (after sending)
    - \( P3 = 0 \)

7. \( P3 \) receives the message, updates its clock to \( \text{max}(0, 5) + 1 = 6 \):
    - \( P1 = 2 \)
    - \( P2 = 5 \)
    - \( P3 = 6 \)

## Vector Algorithm
A **vector clock** is an extension of Lamport's logical clock, designed to 
track causality more precisely in distributed systems. While Lamport’s 
clock can order events partially, it cannot determine if two events are 
concurrent (i.e., unrelated). Vector clocks solve this by providing a 
mechanism that allows processes to determine the causal relationship 
between events, including when events are independent.

### Working Principle
1. **Initialization:** Every Process in the system initializes its vector clock
with all entries set to 0.
2. **Event at a Process:** The process increments its own entry in its
vector clock.
3. **Sending a Message:**
- Before sending a message, the process increments its own entry in its
vector clock.
- It then sends the message with its current vector clock.
4. **Receiving a Message:**
- Upon receiving a message, it updates it's own vector clock. It takes
the maximum of the value in its own vector clock and the value in the received vector
clock.
- It then increments its own entry in its vector clock.

### Comparing Vector Clocks:
- A vector \( V1 \) is **less than** another vector \( V2 \) (i.e., 
\( V1 < V2 \)) if and only if each entry in \( V1 \) is less than or equal 
to the corresponding entry in \( V2 \), and at least one entry is strictly 
less. This means \( V1 \) causally happens before \( V2 \).
- If neither \( V1 < V2 \) nor \( V2 < V1 \), the events are **concurrent** 
(i.e., they have no causal relationship).

### Example:

Let’s assume a distributed system with 3 processes: \( P1 \), \( P2 \), 
and \( P3 \). The vector clocks are initially:

- \( VC1 = [0, 0, 0] \) at \( P1 \)
- \( VC2 = [0, 0, 0] \) at \( P2 \)
- \( VC3 = [0, 0, 0] \) at \( P3 \)

Now, we illustrate a sequence of events:

1. **Event at \( P1 \)**: \( P1 \) performs a local event, so it increments 
its own clock:
   - \( VC1 = [1, 0, 0] \)
   - \( VC2 = [0, 0, 0] \)
   - \( VC3 = [0, 0, 0] \)

2. **Event at \( P2 \)**: \( P2 \) performs a local event:
   - \( VC1 = [1, 0, 0] \)
   - \( VC2 = [0, 1, 0] \)
   - \( VC3 = [0, 0, 0] \)

3. **Message from \( P1 \) to \( P2 \)**: \( P1 \) sends a message to \( P2 
\) and increments its clock:
   - \( VC1 = [2, 0, 0] \) (after sending)
   - \( P1 \) sends its vector clock \( [2, 0, 0] \) to \( P2 \).

   When \( P2 \) receives the message, it updates its clock by taking the 
element-wise maximum:
   - \( VC2 = \text{max}([0, 1, 0], [2, 0, 0]) = [2, 1, 0] \)

4. **Event at \( P3 \)**: \( P3 \) performs a local event:
   - \( VC1 = [2, 0, 0] \)
   - \( VC2 = [2, 1, 0] \)
   - \( VC3 = [0, 0, 1] \)

5. **Message from \( P2 \) to \( P3 \)**: \( P2 \) sends a message to \( P3 
\), increments its clock, and sends \( [2, 2, 0] \):
   - \( VC2 = [2, 2, 0] \) (after sending)
   - \( VC3 \) receives the message and updates its clock:
   - \( VC3 = \text{max}([0, 0, 1], [2, 2, 0]) = [2, 2, 1] \)

### Final Vector Clocks:
- \( VC1 = [2, 0, 0] \)
- \( VC2 = [2, 2, 0] \)
- \( VC3 = [2, 2, 1] \)

### Causal Relationships:
To determine the order of two events from their vector clocks:
- If all the entries in vector clock A are less than or equal to the 
entries in vector clock B, then A happened-before B.
- If all the entries in vector clock A are greater than or equal to the 
entries in vector clock B, then B happened-before A.
- If some entries in vector clock A are less than the corresponding 
entries in vector clock B and some entries are greater, then A and B 
are concurrent events.

### Explanation
Let's compare \( VC1 = [2, 0, 0] \) and \( VC3 = [2, 2, 1] \):
- \( VC1 \) is not less than \( VC3 \) because \( 0 < 2 \) and \( 0 < 1 \) 
for the second and third entries.
- \( VC3 \) is not less than \( VC1 \) because \( 2 = 2 \) in the first 
entry, but for the second and third entries, \( VC3 \) is greater than 
\( VC1 \).

Thus, neither \( VC1 < VC3 \) nor \( VC3 < VC1 \), which means that
**the events at \( P1 \) and \( P3 \) are concurrent**.


