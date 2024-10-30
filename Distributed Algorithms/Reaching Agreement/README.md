# Reaching Agreement

## Byzantine Generals Problem

The Byzantine Generals Problem models the challenge of reaching consensus in a distributed system with some unreliable or malicious participants (traitors). The setup involves generals who must agree on a single plan (e.g., "attack" or "retreat") to succeed. However, some generals may send conflicting messages or none at all, creating ambiguity.

The goal is for all loyal participants to agree on the same course of action, even with potential traitors, ensuring:

1. **Agreement:** All loyal generals reach the same decision.
2. **Validity:** If all loyal generals receive the same command, they follow it.
3. **Fault Tolerance:** Consensus should be possible even with up to m faulty members.

## Oral Message Algorithm by Lamport, Shostak, and Pease

The Oral Message Algorithm by Lamport, Shostak, and Pease is a classic algorithm for reaching agreement in distributed systems, even in the presence of faulty processes. This algorithm was developed to address the Byzantine Generals Problem, which describes the difficulties in reaching consensus when some participants may act maliciously or inconsistently.

The **Oral Message Algorithm** by Lamport, Shostak, and Pease is a classic algorithm for reaching agreement in distributed systems, even in the presence of faulty processes. This algorithm was developed to address the _Byzantine Generals Problem_, which describes the difficulties in reaching consensus when some participants may act maliciously or inconsistently.

The algorithm has two versions based on the number of faulty processes it can handle:

1. **OM(0)**: Handles zero faulty processes.
2. **OM(m)**: Handles up to _m_ faulty processes.

The algorithm is particularly useful in a situation where participants need to reach consensus despite the possibility of some members lying or sending conflicting information. Here’s a breakdown of the Oral Message (OM) Algorithm.

### Working Mechanism of the OM(m) Algorithm

To tolerate up to _m_ faulty processes, the algorithm follows these steps:

1. **Initialization**: The process that wants to send a command is designated as the "commander," and the other processes are "lieutenants."
2. **Message Propagation**:
   - The commander sends its message to each lieutenant.
   - Each lieutenant, upon receiving the message, relays it to the other lieutenants, continuing for up to _m_ levels (rounds) of messaging.
3. **Recursive Function Call**:
   - Each process collects messages from all other processes through recursive calls, simulating message paths and handling up to _m_ faults by re-evaluating the message through intermediaries.
4. **Decision by Majority**:
   - After _m + 1_ rounds, each lieutenant uses majority voting to decide on the command.
   - If there is a majority (of consistent messages), it decides based on that; otherwise, it defaults to a predefined value (like "RETREAT" if commands are "ATTACK" or "RETREAT").

### Example of OM(1) Algorithm with One Faulty Process

Assume there are **4 processes** in the system: **Commander C** and **Lieutenants L1, L2, and L3**. Let's say **L2** is faulty.

#### Scenario

- Commander C needs to decide whether to "ATTACK" or "RETREAT."
- C sends the command "ATTACK" to all lieutenants.
- Since we have **OM(1)**, the algorithm assumes that up to **1 faulty process** may exist.

#### Step-by-Step Execution of OM(1)

1. **Round 1 - Commander Sends Message**:

   - Commander C sends the message "ATTACK" to each lieutenant (L1, L2, and L3).
   - **Faulty Process L2** may send a different command, say "RETREAT," to the others.

2. **Round 2 - Lieutenants Forward the Message**:

   - Each lieutenant forwards the message they received to the other lieutenants.
     - **L1** receives "ATTACK" from C and forwards "ATTACK" to L2 and L3.
     - **L2** (faulty) might forward "RETREAT" to L1 and L3.
     - **L3** receives "ATTACK" from C and forwards "ATTACK" to L1 and L2.

3. **Majority Voting**:

   - After receiving all messages, each lieutenant applies majority voting to decide on the command.
     - **L1** has received two "ATTACK" messages (from C and L3) and one "RETREAT" (from L2), so L1 chooses "ATTACK."
     - **L3** also received two "ATTACK" messages (from C and L1) and one "RETREAT" (from L2), so L3 chooses "ATTACK."
     - **L2**, being faulty, may still choose "RETREAT."

4. **Final Decision**:
   - The majority decision among the non-faulty lieutenants (L1 and L3) is "ATTACK."
   - Even though L2 might have chosen "RETREAT," L1 and L3 achieve consensus on "ATTACK," achieving the majority agreement.
