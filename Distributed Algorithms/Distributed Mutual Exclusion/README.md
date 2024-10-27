# Distributed Mutual Exclusion

## Suzuki-Kasami algorithm

The Suzuki-Kasami algorithm is a distributed mutual exclusion algorithm
designed for systems where multiple processes need exclusive access to a
shared resource in a distributed environment. The algorithm is token-based
and guarantees that only one process at a time can access the critical
section.

### Working Mechanism

**1. Working Mechanism:**

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

### Working Mechanism:

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
