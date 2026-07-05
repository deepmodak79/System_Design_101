# System Design Notes

---

# Load Balancing - Round Robin

Round Robin is a load balancing algorithm that distributes incoming requests across multiple servers in sequence.

Example:

Request 1 -> Server 1

Request 2 -> Server 2

Request 3 -> Server 3

Request 4 -> Server 1

...

### Problem with Sessions

Suppose a user logs in through **Server 1**.

If the session data is stored only on Server 1 and the next request is routed to **Server 2**, then Server 2 cannot find the user's session.

Result:
- User may be logged out.
- User may need to authenticate again.

This problem occurs because the session is stored locally on one server.

---

# Sticky Sessions

Sticky Sessions (Session Affinity) ensure that once a user connects to a server, all future requests from that user are routed to the same server.

Example:

User A
↓
Load Balancer
↓
Server 1

Every future request from User A goes to Server 1.

### Advantages

- Easy to implement.
- No shared session storage required.

### Disadvantages

- Poor load balancing.
- If the server crashes, the user's session is lost.
- Difficult to scale horizontally.

---

# Shared Session Storage

Instead of storing sessions inside individual servers, store them in a centralized location.

Example technologies:

- Redis
- Memcached
- MySQL
- NFS
- SAN (Storage Area Network)

Now every application server can access the same session data.

Flow:

User
↓
Load Balancer
↓
Any Server
↓
Shared Session Storage

This removes the need for Sticky Sessions.

---

# Cookies vs Sessions

## Cookies

- Stored on the client's browser.
- Small amount of data.
- Sent with every HTTP request.
- Can store a Session ID.

## Sessions

- Stored on the server.
- Usually identified using a Session ID stored inside a cookie.
- More secure than storing user data directly in cookies.

---

# RAID (Redundant Array of Independent Disks)

RAID combines multiple hard disks to improve:

- Performance
- Fault tolerance
- Availability

Different RAID levels provide different trade-offs.

## RAID 0

- Disk Striping
- Fastest performance
- No redundancy
- If one disk fails, all data is lost.

Use Case:
- Temporary data
- Gaming
- High-speed workloads

---

## RAID 1

- Disk Mirroring
- Two identical copies of data
- If one disk fails, the other still contains all data.

Advantages:

- High reliability
- Easy recovery

Disadvantages:

- 50% storage efficiency.

Example:

2 × 1TB disks

Usable Storage = 1TB

---

## RAID 5

- Striping + Distributed Parity
- Minimum 3 disks.
- Can tolerate one disk failure.
- Good balance of performance and reliability.

---

## RAID 6

- Similar to RAID 5
- Uses double parity.
- Minimum 4 disks.
- Can survive two disk failures.

---

## RAID 10 (1+0)

Combination of:

- RAID 1 (Mirroring)
- RAID 0 (Striping)

Provides:

- High performance
- High availability

Requires at least 4 disks.

Used in high-performance database servers.

---

# Caching

Caching stores frequently accessed data in memory to reduce database queries and improve response time.

Benefits:

- Lower latency
- Reduced database load
- Faster response time

Popular caching systems:

- Redis
- Memcached

Note:

Older versions of MySQL had a Query Cache, but it has been removed in MySQL 8. Modern systems prefer Redis or Memcached.

---

# Multiple Databases

Large systems often use multiple databases instead of a single database.

Reasons:

- Better scalability
- High availability
- Fault tolerance

Common techniques:

## Replication

One Primary database

↓

Multiple Read Replicas

Writes go to the Primary.

Reads can be distributed among replicas.

---

## Sharding

Split data across multiple databases.

Example:

Database 1 → Users A–H

Database 2 → Users I–P

Database 3 → Users Q–Z

This allows the system to handle much larger datasets.
