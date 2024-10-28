# Global Snapshots

## Global Snapshot Algorithm

A Global Snapshot algorithm is used in distributed systems to capture the state of the entire system at a particular point in time, including the states of all processes and the messages in transit between them. This algorithm is essential for debugging, monitoring, and ensuring the consistency of distributed systems.

### Working steps

**1. Marker Introduction:** Initiate the global snapshot by introducing a marker into the system.

> What is a Marker?

> In the context of the Global Snapshot algorithm, a "marker" is a special message or
> signal that is introduced into the distributed system to indicate the point in time at
> which the global snapshot is to be captured.

**2. Local State Recording:** When a process receives the marker, it records its local state(variables, data, etc.) and its empty message queue. Then, it forwards the marker to
neighboring processes.

**3. Marker Collection:** The marker eventually reaches the collector process. Upon receiving the marker, the collector records its local state, which includes variables and the message queue.

**4. Global Snapshot:** The collector combines the recorded states and message queues from all processes to create a global snapshot, reflecting the entire system's state at that specific moment.

## The Chandy-Lamport Snapshot algorithm

The Chandy-Lamport Snapshot Algorithm is a classic distributed system algorithm designed to capture a consistent global state of an asynchronous distributed system without pausing its execution.

### Working Mechanism

The algorithm operates by taking "snapshots" of each process’s state and the messages in transit across the channels, using **marker messages** to control when snapshots are taken. Here’s how it works:

1. **Initiation**: A process initiates the snapshot procedure by recording its own state and sending a special **marker message** through all outgoing channels to other processes.

2. **Receiving a Marker**:

   - If a process receives a marker message and hasn’t recorded its state yet, it records its state, then sends a marker message on all its outgoing channels.
   - If it has already recorded its state (i.e., it already received a marker), it only records the state of the incoming channel as the messages received after the marker message was received.

3. **State Recording**:

   - Each process records its own local state.
   - For each channel, a process records the messages that were "in transit" when the snapshot was taken. This means messages that were sent before the marker was received but were not yet received by the process.

4. **Completion**: The snapshot is complete when each process has recorded its state and the state of all incoming channels.

### Example

Consider a distributed system with three processes, \( P1 \), \( P2 \), and \( P3 \), and channels connecting them. Let’s assume \( P1 \) initiates the snapshot.

1. **Step 1 - Initiation**:

   - \( P1 \) records its own state (e.g., local variables, current execution state).
   - \( P1 \) sends a **marker** on its outgoing channels to \( P2 \) and \( P3 \).

2. **Step 2 - Receiving Marker at \( P2 \)**:

   - \( P2 \) receives the marker from \( P1 \) on the channel \( C\_{P1 \to P2} \).
   - Since this is the first marker \( P2 \) has received, it records its own state.
   - \( P2 \) then sends markers on its outgoing channels to \( P1 \) and \( P3 \).
   - Any messages \( P2 \) receives from \( P1 \) after recording its state but before receiving the marker on that channel are considered in-transit.

3. **Step 3 - Receiving Marker at \( P3 \)**:

   - \( P3 \) receives a marker from \( P1 \) on \( C\_{P1 \to P3} \).
   - Like \( P2 \), it records its state and sends markers on its outgoing channels to \( P1 \) and \( P2 \).
   - Any messages that \( P3 \) receives on any channel before it gets a marker on that channel are in-transit messages.

4. **Step 4 - Completion**:
   - After all processes have received a marker on all incoming channels and recorded their states, the snapshot is complete.
   - The global state is the combination of the individual states and the in-transit messages recorded for each channel.
