# Message Ordering
## Birman-Schiper-Stephenson (BSS)
The Birman-Schiper-Stephenson (BSS)protocol, also known as the Causal 
Broadcast algorithm, is designed to ensure that messages in a distributed 
system are delivered in causal order. Causal order ensures that if a 
message \( m1 \) causally precedes another message \( m2 \), then every 
process in the system that delivers both messages will deliver \( m1 \) 
before \( m2 \).

### Working Principle
- A vector clock (like in the vector clock algorithm), where each element 
represents the process's knowledge of the latest event that occurred in 
every other process.
- A pending message queue to hold messages that cannot yet be delivered 
because earlier messages in the causal order haven’t been received yet.
- When a process sends a message
𝑚, it increments its own vector clock and sends the message along with 
its current vector clock (dependency vector).
- This dependency vector describes the causal history of the process at 
the time the message is sent.
- When a process P(i) receives a message from process P(j), it checks the vector
clock attached to the message to determine whether all casually preceding
messages have been received.
- If causally preceding messages are still missing, the message is stored in
the pending message queue.
- Once all causally preceding messages have been delivered, the message
can be delivered.

### Example:

Assume we have three processes \( P1 \), \( P2 \), and \( P3 \) that are 
communicating with each other using the BSS algorithm. The vector clocks 
are used to keep track of causality, with each process maintaining a 
vector clock \( VC_i \) for its causal knowledge of events.

1. **Initialization**:
    - All processes start with \( VC1 = [0, 0, 0], VC2 = [0, 0, 0], 
   VC3 = [0, 0, 0] \).

2. **Event at \( P1 \)**:
    - \( P1 \) performs a local event, increments its clock to \( VC1 = 
   [1, 0, 0] \), and sends a message \( m1 \) to \( P2 \).
    - The message \( m1 \) is sent with the vector clock \( [1, 0, 0] \).

3. **Receiving at \( P2 \)**:
    - \( P2 \) receives the message \( m1 \) and checks its own vector 
   clock, \( VC2 = [0, 0, 0] \).
    - Since \( VC1 = [1, 0, 0] \) is greater in the first component than 
   \( VC2 \), it delivers the message and updates \( VC2 = [1, 0, 0] \) 
   to reflect that it has received the message from \( P1 \).

4. **Event at \( P2 \)**:
    - \( P2 \) performs a local event, increments its vector clock to \
   ( VC2 = [1, 1, 0] \), and sends a message \( m2 \) to \( P3 \) with 
   vector clock \( [1, 1, 0] \).

5. **Receiving at \( P3 \)**:
    - \( P3 \) receives \( m2 \) from \( P2 \). Its own clock is 
   \( VC3 = [0, 0, 0] \).
    - \( VC3 \) sees that it has not yet received the message from 
    \( P1 \) (because \( VC2[0] = 1 \) but \( VC3[0] = 0 \)), so it 
   cannot deliver \( m2 \) yet.
    - \( P3 \) places the message in its pending queue until it 
   receives all messages that causally precede \( m2 \).

6. **Receiving from \( P1 \) at \( P3 \)**:
    - \( P3 \) later receives \( m1 \) from \( P1 \), with vector 
   clock \( [1, 0, 0] \).
    - It delivers \( m1 \) and updates its clock to \( VC3 = [1, 0, 0] \).
    - Now that \( P3 \) has received \( m1 \), it can deliver \( m2 \) 
   from \( P2 \), as the causal dependency is satisfied.

## Schiper-Egli-Sandoz
The Schiper-Egli-Sandoz (SES) algorithm is a causal ordering protocol 
that ensures messages in distributed systems are delivered respecting 
their causal relationships.

### Working Mechanism
1. **Initialization:**
Each process in Schiper-Egli-Sandoz algorithm uses logical timestamps to 
mark events and track only necessary dependencies. When a process sends a 
message, it tags it with a timestamp and a list of dependencies, capturing 
the state of the process at the time of sending.
2. **Checking Dependencies on Receiving:**
- Checks the dependency information to ensure that all messages it depends 
on have been received.
- Delays the delivery of a message if any of its dependencies are missing, 
placing it in a pending queue.
- Only delivers the message once all dependencies have been satisfied.

### Example of Message Delivery:
Suppose we have three processes, \( P1 \), \( P2 \), and \( P3 \), and 
they are communicating through the SES algorithm. Here's how the 
algorithm would ensure causal delivery:

1. **Initialization**:
   - All processes start with their own logical clocks set to zero.

2. **Event at \( P1 \)**:
   - \( P1 \) sends a message \( m1 \) to \( P2 \) and attaches a timestamp 
   or minimal dependency information.

3. **Receiving \( m1 \) at \( P2 \)**:
   - \( P2 \) receives \( m1 \) from \( P1 \) and checks if there are any 
   missing dependencies. Since \( m1 \) is the first message, it has no dependencies and can be delivered immediately.

4. **Event at \( P2 \)**:
   - \( P2 \) performs an action after receiving \( m1 \) and sends a new 
   message \( m2 \) to \( P3 \), tagging it with a dependency on \( m1 \) 
   (since \( m2 \) causally depends on \( m1 \)).

5. **Receiving \( m2 \) at \( P3 \)**:
   - When \( P3 \) receives \( m2 \), it checks if it has already received 
   \( m1 \) from \( P1 \), based on the dependency information tagged with 
   \( m2 \).
   - If \( m1 \) has not yet been received, \( m2 \) is placed in the 
   pending queue until \( m1 \) is received, thus preserving the causal 
   order.
   - Once \( m1 \) is received, \( P3 \) delivers both \( m1 \) and 
   \( m2 \) in the correct causal order.