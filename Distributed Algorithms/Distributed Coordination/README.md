# Distributed Coordination

## Bully Algorithm

The Bully Algorithm is a leader election algorithm used in distributed systems to
elect a coordinator or leader among a group of processes. The coordinator is typically
the process with the highest priority or ID, and it takes charge of centralized tasks
within the distributed system.

## Working Mechanism

1. **Election Trigger**: Any process can initiate the election if it notices the current coordinator is down or if it itself needs a new coordinator (e.g., when it starts up).

2. **Election Messages**: The initiating process sends an "election" message to all other processes with higher IDs.

3. **Responses**:

   - If a process with a higher ID receives an election message, it replies to indicate it's still active and will take over the election.
   - The higher-ID process then starts a new election with all processes that have higher IDs than itself.

4. **Coordinator Election**:

   - If a process receives no responses from higher-ID processes, it declares itself the coordinator.
   - It then sends a "coordinator" message to all other processes, informing them that it is now the new leader.

5. **Failure Handling**: If the current coordinator fails, any other process noticing the failure can initiate a new election. This keeps the system fault-tolerant.

### Example

Consider five processes with IDs 1, 2, 3, 4, and 5 in a distributed system. Process 5 is the coordinator, but it crashes. Now, let’s see how the Bully Algorithm works in this scenario:

1. **Election Trigger**: Process 3 notices the coordinator (Process 5) has crashed and starts an election by sending an election message to Processes 4 and 5 (all with higher IDs).

2. **Responses**:

   - Process 4, still active, receives the message from Process 3 and responds, indicating it will now start an election of its own with higher-ID processes (only Process 5 in this case).
   - Since Process 5 is down, Process 4 doesn’t receive any response back.

3. **Coordinator Declaration**:

   - Process 4, receiving no responses from higher-ID processes, declares itself as the coordinator.
   - It sends a "coordinator" message to all other processes (1, 2, and 3), announcing itself as the new leader.

4. **System Stabilization**: Now, all processes know Process 4 is the coordinator. They resume their tasks with Process 4 as the leader.

## Lelann's Ring Algorithm

Lelann's **Ring Algorithm** is a leader-election algorithm used in distributed systems where processes are arranged in a logical ring. This algorithm works by passing a token around the ring to identify the highest-ID process, which becomes the new coordinator or leader.

### Working Mechanism of Lelann's Ring Algorithm

1. **Ring Setup**: Processes in the system are organized in a logical ring. Each process only knows its immediate neighbor's address in the ring (either clockwise or counterclockwise).

2. **Election Trigger**: Any process can initiate an election if it detects the coordinator has failed. This initiating process sends an "election" message, including its ID, to its neighbor in the ring.

3. **Passing the Token**:

   - Each process receiving the election message compares its ID with the ID in the message.
   - If the ID in the message is higher, the process forwards the message (token) unchanged to the next process in the ring.
   - If the receiving process has a higher ID than the one in the token, it replaces the token’s ID with its own and forwards it.

4. **Coordinator Selection**:
   - The election message circulates around the ring until it returns to the initiating process.
   - By the time the message completes a full loop, it contains the highest ID in the ring.
   - The process with the highest ID declares itself as the coordinator and sends a "coordinator" message around the ring, informing all other processes.

### Example

Consider a distributed system with five processes, labeled with IDs 1, 2, 3, 4, and 5, arranged in a ring (e.g., 1 → 2 → 3 → 4 → 5 → 1). Suppose the current coordinator (Process 5) has crashed, and Process 2 initiates a new election.

1. **Election Trigger**: Process 2 detects the failure and starts the election by sending an election message with its own ID (2) to Process 3.

2. **Token Passing and ID Comparison**:

   - Process 3 receives the token with ID 2. Since 3 > 2, it updates the token to ID 3 and forwards it to Process 4.
   - Process 4 receives the token with ID 3. Since 4 > 3, it updates the token to ID 4 and forwards it to Process 5.
   - Process 5 is down, so the token skips over it to Process 1.
   - Process 1 receives the token with ID 4. Since 4 > 1, it forwards the token unchanged to Process 2, completing the loop.

3. **Coordinator Declaration**:

   - When the token returns to Process 2, it contains ID 4, indicating Process 4 has the highest ID in the ring.
   - Process 4 declares itself the new coordinator and sends a "coordinator" message around the ring, announcing its role to all other processes.

4. **System Stabilization**: Once all processes receive the "coordinator" message, they recognize Process 4 as the new coordinator, and the system resumes its operations.

## Peterson's Ring Algorithm

**Peterson's Ring Algorithm** is another leader-election algorithm for distributed systems. Similar to Lelann’s Ring Algorithm, processes are arranged in a logical ring. Unlike LeLann's ring structure where the messages travel in a ring structure, in the Peterson's algorithm it handles the message passing in a tree like structure.

### Working Mechanism

1. It starts from a initial node. There are two types of nodes.
   - One is **active** nodes who participates in the election.
   - Second ones are relay nodes, just passes the message.
2. In terms of traveling through the graph it travels in a bidirectional or uni directional way.
3. When going into bidirection it compares itself with the elements in left, right and itself.
4. In terms of uni-directional way it only checks two predecessor elements with itself.
5. It cuts the steps in half in every iteration

### Example

Let's go one by one. It first step:

1. First iteration.
   - It starts with 4. 4 sees in the left and right it is the biggest item. It makes 1 and 2 passive. Then it takes two steps forward.
   - It goes to 50, the situation is the same. It 50 makes 0 and 8 passive.
   - It then takes two steps forward and goes to 7 and sees that, left one (11) is bigger. 11 then stays active whereas 7 and 9 become unactive.
   - After two steps ahead it goes to 17 it sees only in the left 9 which is already in passive mode.
   - Total steps 4.
2. Second iteration:
   - It stats with 4, it looks two of it's predecessor 17 and 11. Since, 17 is the biggest, 11 and 4 becomes passive.
   - It then goes to 17, sees 11 in left which is passive mode.
   - total step 2.
3. Third iteration:
   - It stats with 50 and in the right it only sees 17 which is inactive, which makes 50 the highest process id.
   - total step 1.
