# Chapter-01
## Definition
A distributed system is a network of independent computers that 
work together as a single system to accomplish shared tasks. 
These computers communicate and coordinate their actions by 
passing messages to one another.


Why build distributed systems:

1. Scalability: Handle larger workloads by adding more machines.
2. Fault tolerance: Continue functioning even if some components fail.
3. Latency reduction: Serve users from geographically closer locations.
4. Resource sharing: Utilize collective computational power and storage.

How to build distributed systems:

1. Choose an architecture (e.g., client-server, peer-to-peer, microservices)
2. Implement communication protocols (e.g., HTTP, gRPC, message queues)
3. Ensure data consistency (e.g., consensus algorithms, distributed transactions)
4. Design for fault tolerance (e.g., replication, load balancing)
5. Monitor and manage the system (e.g., logging, tracing, metrics)

## Peer-2-Peer transactions.
Peer-to-peer (P2P) transactions are direct exchanges between individuals 
without an intermediary. They allow people to transfer money, goods, or 
services directly to each other. P2P systems distribute tasks among peers,
who are both suppliers and consumers of resources.

- **Example:** Bitcoin is a P2P network where users can send digital currency (Bitcoins) directly to
  each other's digital wallets without needing a bank or payment service. Transactions are recorded on
  a public ledger called the blockchain, ensuring transparency and security.

## Distributed Systems vs Decentralized Systems
Key Differences:
1. **Control:** Distributed systems may have centralized control; 
decentralized systems avoid it.
2. **Purpose:** Distributed systems focus on efficiency; decentralized 
systems prioritize autonomy.
3. **Architecture:** Distributed systems can be hierarchical; decentralized 
systems are usually flat.
4. **Trust:** Distributed systems often assume trusted components; 
decentralized systems are designed for trustless environments.

Example:
A company's cloud infrastructure is a distributed system - servers are 
spread out but centrally managed. Bitcoin, on the other hand, is both 
distributed and decentralized - it's spread across many computers with 
no central authority.

## Ubiquitous Computing
Ubiquitous computing, also known as pervasive computing, is the concept 
of embedding computational capability into everyday objects and 
environments, making technology seamlessly integrated into our 
daily lives. It aims to make computing available anywhere and anytime, 
often in ways that are invisible to the user.

A smart home environment demonstrates ubiquitous computing in action. 
Here's how it might work:

1. As you approach your house, your smartphone communicates with the home 
system.
2. The doors unlock automatically when you reach them.
3. Lights turn on as you enter each room, adjusting brightness based on 
time of day and ambient light.
4. The thermostat adjusts temperature according to your preferences and 
presence.
5. Your refrigerator notices you're low on milk and adds it to your 
shopping list.
6. As you sit on the couch, the TV turns on and suggests shows based on 
your viewing history.
7. When you go to bed, the system automatically locks doors, turns off 
lights, and sets the alarm.

## Edge Computing and Fog Computing

Definitions:

Edge computing and fog computing both aim to bring computation closer to where data is generated
to reduce latency and improve efficiency. Edge computing focuses on ultra-low latency and
immediate decision-making, while fog computing adds a layer of intermediate processing for more
complex tasks and localized data analysis. Both paradigms are important for the growing Internet of
Things (IoT) ecosystem and real-time applications.

Key Differences:

1. Location: Edge operates directly on or near source devices; fog 
operates in a layer between edge and cloud.
2. Scope: Edge typically processes data on individual devices; fog 
covers a wider network of devices.
3. Latency: Edge offers lowest latency; fog provides low latency 
but slightly higher than edge.
4. Architecture: Edge is more decentralized; fog creates a hierarchical 
structure.
5. Data handling: Edge often works with raw data; fog typically handles 
pre-processed data.

## Benefits, Limitations and Dependencies of Distributed Systems
BENEFITS:

1. Scalability
- Horizontal scaling by adding more machines
- Handles increased load and data growth
- Easy to expand resources as needed

2. Performance
- Parallel processing capabilities
- Load balancing across multiple nodes
- Reduced response time through distributed processing

3. Reliability & Availability
- System continues functioning despite partial failures
- High availability through redundancy
- No single point of failure

4. Resource Sharing
- Shared hardware and software resources
- Cost-effective utilization of resources
- Better resource management

5. Flexibility
- Easy to add or remove components
- Support for heterogeneous systems
- Geographic distribution possible

LIMITATIONS:

1. Complexity
- Difficult to design and maintain
- Complex debugging and testing
- Requires specialized expertise

2. Security Challenges
- Multiple points of attack
- Complex security protocols needed
- Data vulnerability during transit

3. Network Issues
- Network failures can affect system
- Bandwidth limitations
- Network latency problems

4. Consistency Challenges
- Maintaining data consistency across nodes
- Complex synchronization requirements
- CAP theorem trade-offs

5. Cost
- Infrastructure setup costs
- Maintenance expenses
- Network and communication costs

DEPENDENCIES:

1. Network Infrastructure
- Reliable network connectivity
- Sufficient bandwidth
- Network protocols and standards

2. Hardware
- Compatible hardware systems
- Processing capacity
- Storage requirements

3. Software
- Operating systems
- Middleware
- Communication protocols

4. Data Management
- Database systems
- Data synchronization tools
- Backup systems

5. Security Infrastructure
- Authentication systems
- Encryption mechanisms
- Security protocols

6. Management Tools
- Monitoring systems
- Administration tools
- Logging and tracking systems

## Location Dependent and Location Independent Services
**Location-Independent Services:**

- Definition: Digital services accessible from anywhere, not tied to a specific location.
- Examples: Cloud email, social media, online collaboration tools.
- Comparison: Work the same for users everywhere, not influenced by location.

**Location-Based Services:**

- Definition: Digital services that use a user's location to provide specific information or
functionality.
- Examples: GPS navigation apps, local business finders, geofencing notifications.
- Comparison: Tailor content and services based on the user's current geographic location.

## Synchronous System and Asynchronous System
### Synchronous System
* Systems where operations are coordinated by a global clock
* Each step occurs at fixed, predictable time intervals
* Processes have strict timing constraints

> A distributed system is called synchronous when: There exists a system-wide known upper limit
for the divergence of local clocks between different nodes of the system.
- Clock Divergence: Each node in the system has it's own clock.
- Upper Limit: There is a maximum known value for this divergence.
- System Wide Knowledge: All nodes know this upper limit.

: 