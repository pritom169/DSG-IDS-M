# Distributed Mutual Exclusion

## Suzuki-Kasami algorithm

The Suzuki-Kasami algorithm is a distributed mutual exclusion algorithm
designed for systems where multiple processes need exclusive access to a
shared resource in a distributed environment. The algorithm is token-based
and guarantees that only one process at a time can access the critical
section.

### Working Mechanism

**1. Intialization:**

- One process starts with the token.
- Every process maintains a request queue of processes that have requested the token
  but haven't yet received it.
- Every process also maintains a number called its "request number", initialized to 0.
  It represents the number of times the process has requested access to the critical
  section.

**2. Requesting the Critical Section:**

- When a process wants to enter the critical section and doesn't have the token, it
  increases its request number by 1.
- It then broadcasts a request message to all other processes, containing its process ID and its updated request number.

**3. Receiving a Request Message:**

- Upon receiving a request message, a process updates its request queue and sends a
  timestamped acknowledgment back to the requester, letting the requester know it
  received the request. This timestamped acknowledgment is essentially the receiver's
  current request number for the token.
  **4. Receiving the Token:**

- When a process receives the token, it can enter the critical section.
- Once it exits the critical section, it checks its request queue to see which process
  should receive the token next. The token is then sent to the next process in the queue
  that has the highest request number.

### Example:

- **Token Holder**: \( P1 \) initially holds the token.
- **Request Numbers**: All processes start with \( RN[i] = 0 \) for themselves and all other processes.

#### Steps:

1. **Request by \( P2 \)**:

   - \( P2 \) wants to enter the critical section.
   - It increments its request number to \( RN[2] = 1 \) and broadcasts a request to \( P1 \) and \( P3 \).

2. **Response to \( P2 \)’s Request**:

   - \( P1 \) receives the request from \( P2 \).
   - Since \( P1 \) has the token and is not using the critical section, it sends the token to \( P2 \).
   - \( P3 \) also receives \( P2 \)’s request and updates \( RN[2] \) but does nothing further as it does not hold the token.

3. **\( P2 \) Enters and Releases Critical Section**:

   - \( P2 \) receives the token from \( P1 \) and enters the critical section.
   - Once \( P2 \) finishes, it updates the token’s last processed request number for itself and checks for any pending requests in the queue.
   - If no other requests are pending, \( P2 \) retains the token.

4. **Request by \( P3 \)**:
   - \( P3 \) now wants access, increments \( RN[3] \) to 1, and sends a request to \( P1 \) and \( P2 \).
   - \( P2 \) has the token but is currently not using it, so it sends the token to \( P3 \).
   - \( P3 \) receives the token and can enter the critical section.

## Lamport's Algorithm

Lamport's algorithm is a famous approach for achieving mutual exclusion in a
distributed system. It's based on logical clocks and timestamps. The algorithm
ensures that only one process can enter a critical section at a time.

### Working Mechanism

- Each process maintains a logical clock that keeps track of the order of events.
- When a process wants to enter a critical section, it sends a request to all other processes with its current timestamp.
- It waits until it has received replies from all other processes, and it can only enter the critical section when it has the highest timestamp among all processes.

### Example

Each process has:

- A logical clock initialized to zero.
- A empty request queue.

1. **\( P1 \) Requests the Critical Section**:

   - \( P1 \) wants access, increments its logical clock to 1, and sends a request \((1, P1)\) to \( P2 \) and \( P3 \).
   - \( P1 \) adds its own request to its queue.

2. **\( P2 \) Requests the Critical Section**:

   - \( P2 \) also wants access, increments its clock to 2, and sends \((2, P2)\) to \( P1 \) and \( P3 \).
   - \( P2 \) adds \((2, P2)\) to its queue.

3. **Receiving and Replying to Requests**:

   - \( P1 \) and \( P2 \) receive each other’s requests and place them in their local queues.
   - Both \( P1 \) and \( P2 \) receive replies from \( P3 \), as \( P3 \) has no competing request.

4. **Entering the Critical Section**:

   - Since \( P1 \)’s request has the earliest timestamp, \( P1 \) enters the critical section first after receiving replies from both \( P2 \) and \( P3 \).

5. **Releasing the Critical Section**:
   - \( P1 \) finishes, removes \((1, P1)\) from its queue, and sends a release message to \( P2 \) and \( P3 \).
6. **\( P2 \) Accesses the Critical Section**:
   - \( P2 \), now having the earliest timestamp request in the system, enters the critical section next after receiving the release from \( P1 \).

## Ricart-Agarwala algorithm

The **Ricart-Agrawala algorithm** is a distributed mutual exclusion algorithm used in distributed systems to manage access to a shared resource (or critical section) without requiring a central coordinator. It uses a **permission-based** approach, where each process requests permission from all other processes to enter the critical section. It is based on **Lamport's logical clocks** for ordering requests and requires only \( N-1 \) messages per access, making it more efficient than some other distributed mutual exclusion algorithms.

### Algorithm Workflow:

1. **Requesting Access**:

   - When a process wants to enter the critical section:
     - It increments its logical clock and timestamps its request.
     - It sends this timestamped request to all other processes in the system.

2. **Receiving a Request**:

   - When a process receives a request from sender:
     - If receiver is not interested in entering the critical section or senders's request has a lower timestamp than sender's request, it immediately sends a reply , granting permission.
     - If receiver is currently waiting to enter the critical section and has a request with an earlier timestamp than it delays its reply until it has exited the critical section.

3. **Entering the Critical Section**:

   - When a process enters the critical section once it has received permission (replies) from all other processes, indicating that no other process with a higher priority is waiting.

4. **Releasing the Critical Section**:
   - After finishing in the critical section, the process sends release messages to all processes waiting for it, allowing them to proceed if they are next in line.

### Example:

Consider three processes \( P1 \), \( P2 \), and \( P3 \) in a distributed system.

1. **\( P1 \) Requests Access**:
   - \( P1 \) wants to enter the critical section, so it increments its logical clock to 1, timestamps the request, and sends it to \( P2 \) and \( P3 \).
2. **Receiving \( P1 \)'s Request**:

   - \( P2 \) and \( P3 \) receive \( P1 \)'s request. Since neither \( P2 \) nor \( P3 \) is currently waiting to enter the critical section, they both immediately reply, granting permission to \( P1 \).

3. **\( P1 \) Enters and Releases the Critical Section**:

   - \( P1 \), having received replies from \( P2 \) and \( P3 \), enters the critical section.
   - After finishing, \( P1 \) sends a release message to \( P2 \) and \( P3 \) to indicate it has left the critical section.

4. **\( P2 \) Requests Access**:

   - After \( P1 \) releases the critical section, \( P2 \) decides to enter, increments its logical clock, and sends a request to \( P1 \) and \( P3 \).
   - \( P1 \) and \( P3 \) both respond immediately since neither has conflicting requests.

5. **\( P2 \) Enters the Critical Section**:
   - \( P2 \), now with permissions from \( P1 \) and \( P3 \), enters the critical section.
