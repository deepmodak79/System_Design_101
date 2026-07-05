# System Design Fundamentals - Round 1 Notes

## 1. FTP (File Transfer Protocol)

FTP is a standard network protocol used for transferring files between a client and a server.

### Features
- Upload files to a server
- Download files from a server
- Manage files remotely

### Drawbacks
- Data is transferred in plain text
- Usernames and passwords are not encrypted
- Not considered secure for modern applications

---

## 2. SFTP (Secure File Transfer Protocol)

SFTP is a secure file transfer protocol that runs over **SSH (Secure Shell)**.

### Features
- Encrypted communication
- Secure authentication
- Safe file uploads and downloads

### Use Cases
- Uploading website files to production servers
- Secure data exchange between organizations
- Managing remote servers

### FTP vs SFTP

| FTP | SFTP |
|------|------|
| Not encrypted | Encrypted |
| Less secure | Highly secure |
| Uses Port 21 | Uses Port 22 (SSH) |
| Legacy protocol | Modern preferred protocol |

---

## 3. VPS (Virtual Private Server)

A **VPS** is a virtual machine running on a physical server.

### Popular Cloud Providers
- Amazon Web Services (AWS)
- Microsoft Azure
- Google Cloud Platform (GCP)
- DigitalOcean
- Linode

### Benefits
- Dedicated resources
- Better performance than shared hosting
- More control over the environment
- Can host websites, APIs, databases, and applications

---

# 4. Website Scaling

As the number of users increases, the system must scale to handle more traffic.

There are two major approaches:

1. Vertical Scaling (Scale Up)
2. Horizontal Scaling (Scale Out)

---

# 5. Vertical Scaling (Scale Up)

Vertical scaling means increasing the resources of a **single server**.

### Examples
- More RAM
- Faster CPU
- More Storage
- Better Hardware

### Example

**Before**
- 4 GB RAM
- 2 CPU Cores

**After**
- 32 GB RAM
- 8 CPU Cores

### Advantages
- Easy to implement
- No major architectural changes

### Disadvantages
- Hardware limit exists
- Expensive
- Single point of failure

---

# 6. CPU Cores and Parallel Processing

Modern CPUs contain multiple cores.

### Examples
- Dual Core = 2 Cores
- Quad Core = 4 Cores
- Octa Core = 8 Cores

### Quad Core CPU

A Quad Core CPU can execute multiple tasks simultaneously.

Examples include:
- Processing API requests
- Running background jobs
- Database operations
- Handling user sessions

> More CPU cores generally improve parallel processing capability.

---

# 7. Storage Technologies

## PATA (Parallel ATA)

Older storage technology.

### Characteristics
- Parallel data transfer
- Large ribbon cables
- Slower performance

---

## SATA (Serial ATA)

Modern replacement for PATA.

### Characteristics
- Serial communication
- Faster data transfer
- Better cable management
- Common in HDDs and SSDs

---

## SAS (Serial Attached SCSI)

Enterprise-grade storage interface.

### Characteristics
- High reliability
- Better performance
- Used in data centers
- Supports larger workloads

---

# 8. HDD vs SSD

## HDD (Hard Disk Drive)

Mechanical storage device.

### Characteristics
- Moving parts
- Slower read/write speeds
- Lower cost

---

## SSD (Solid State Drive)

Flash-based storage.

### Characteristics
- No moving parts
- Much faster performance
- Lower latency
- Better reliability

### HDD vs SSD

| HDD | SSD |
|------|------|
| Mechanical | Solid State |
| Slower | Faster |
| Cheaper | More Expensive |
| Higher Latency | Lower Latency |

> **For modern web applications and databases, SSDs are preferred.**

---

# 9. DNS (Domain Name System)

DNS translates human-readable domain names into IP addresses.

### Example

```text
google.com
      ↓
142.250.x.x
```

### Why DNS is Needed

Humans remember:

- google.com
- amazon.com

Computers communicate using:

- IP Addresses

> DNS acts as the Internet's phonebook.

### DNS Resolution Flow

```text
User Browser
      ↓
DNS Resolver
      ↓
DNS Server
      ↓
IP Address Returned
      ↓
Browser Connects to Website
```

---

# 10. Horizontal Scaling (Scale Out)

Horizontal scaling means adding **multiple servers** instead of upgrading a single server.

### Example

Instead of:

**1 Server**
- 64 GB RAM

Use:

**4 Servers**
- 16 GB RAM each

### Advantages
- Better fault tolerance
- Higher availability
- Easier growth
- No single server dependency

### Disadvantages
- More complex architecture
- Requires load balancing

---

# 11. Load Balancer

A Load Balancer distributes incoming requests across multiple servers.

### Without Load Balancer

```text
Users
   │
   ▼
Single Server
   │
Server Overload
```

### With Load Balancer

```text
              Users
                │
                ▼
          Load Balancer
          /     |      \
         ▼      ▼       ▼
    Server 1 Server 2 Server 3
```

### Responsibilities
- Distribute traffic
- Detect unhealthy servers
- Improve availability
- Prevent server overload

### Specialized Servers

Traffic can also be routed based on responsibility.

Examples:
- Image Server
- Video Server
- API Server
- Authentication Server
- Static File Server

This improves performance and resource utilization.

---

# 12. Round Robin Algorithm

Round Robin is one of the simplest load balancing strategies.

Requests are distributed sequentially.

### Example

```text
Request 1 → Server A
Request 2 → Server B
Request 3 → Server C
Request 4 → Server A
Request 5 → Server B
Request 6 → Server C
```

### Advantages
- Simple
- Easy to implement
- Fair distribution

### Limitations
- Does not consider current server load
- Assumes all servers have equal capacity

---

# Key Takeaways

- FTP transfers files but is **not secure**.
- SFTP provides **secure encrypted file transfer** using SSH.
- VPS offers dedicated virtual resources for hosting applications.
- Vertical Scaling increases the resources of a single server.
- Horizontal Scaling adds more servers to handle increased traffic.
- SSDs provide significantly better performance than HDDs.
- DNS converts domain names into IP addresses.
- Load Balancers distribute traffic across multiple servers.
- Round Robin is a simple load balancing algorithm.
- Large-scale systems commonly use **Horizontal Scaling + Load Balancers** for high availability and scalability.

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
