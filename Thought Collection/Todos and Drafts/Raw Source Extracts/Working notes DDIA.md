## Chapter 1 - Reliable, Scalable, Maintainable Applications:

### **Q: What benefit can we have with talking about data systems as a terminology?**
His argument is that data systems (databases, queues, caches) all store data for some time. This can be temporarily vs. permanently, they also provide ways to retrieve the data, they accept data as input and have different trade-offs.

Many applications nowadays have so demanding or wide-ranging requirements that a single tool can no longer meet all of its data processing and storage needs. The work is broken down into tasks that can be performed efficiently on a single tool, and those different tools are stitched together using application code. So tasks -> tools -> combining tools to do all that is needed.

Example:
Redis (cache) + Postgres (database) + Kafka (queue) with application code orchestrating them.

This brings forth areas like:
- Consistency across components (if cache and DB diverge, whose truth wins?)
- Failure modes (what happens when Kafka is down but Postgres isn't?)
- Performance characteristics (the composition's latency isn't just sum of parts)

By calling it a "data system" (singular), Kleppmann is saying: **you can't abdicate responsibility to individual tools anymore**.

### **Q: What do wean by Operability? And who's operating? And how?**
**Who's Operating:** 
The **operations team** (often called SRE, DevOps, Platform Engineering, or just Ops). These are the people responsible for keeping your system running in production _after_ you've written the code. In smaller orgs, this might be developers wearing an ops hat. In larger orgs, it's dedicated teams.

In Alektum this responsibility is split and divided causing issues.

**What's Operated:**
Your running system in production - deploying changes, responding to incidents, scaling infrastructure, managing configurations, ensuring uptime.

Good operability means making routine tasks easy, allowing the operations team to focus their efforts on high-value activities. Data systems can do various things to make routine tasks easy, including: 

- Providing visibility into the runtime behavior and internals of the system, with good monitoring 
- Providing good support for automation and integration with standard tools 
- Avoiding dependency on individual machines (allowing machines to be taken down for maintenance while the system as a whole continues running uninterrupted) 
- Providing good documentation and an easy-to-understand operational model (“If I do X, Y will happen”) 
- Providing good default behavior, but also giving administrators the freedom to override defaults when needed
- Self-healing where appropriate, but also giving administrators manual control over the system state when needed 
- Exhibiting predictable behavior, minimizing surprises 

### **Q: Evolvability, what concerns do we have and how do we make sure it can match this criteria?**
Your data system will need to change, but change is expensive/risky/slow. How do you build systems that can adapt without complete rewrites or catastrophic failures?

**Evolvability isn't about predicting the future** - it's about:
1. Reducing the cost of being wrong about requirements
2. Making changes safe through good abstractions and boundaries
3. Enabling experimentation through loose coupling and compatibility
4. Maintaining simplicity so you can reason about changes

**In your org:** Low evolvability is organizational (team silos, gatekeeping) AND technical (tight coupling, shared databases, poor abstractions).

From the citations, a paper written https://www.es.mdh.se/pdf_publications/1251.pdf:
![[Skärmavbild 2025-10-21 kl. 11.48.49.png]]

## Chapter 2 - Data Models & Query Languages

### **What is the different ways we model data?
- Data started out being represented as one big tree (the **hierarchical model**),

- That wasn’t good for representing many-to-many relationships, so the **relational model** was invented to solve that problem.

- More recently, developers found that some applications don’t fit well in the relational model either. New nonrelational “NoSQL” datastores have diverged in two main directions: 
	- **Document databases** target use cases where data comes in self-contained documents and relationships between one document and another are rare. 
	- **Graph databases** go in the opposite direction, targeting use cases where anything is potentially related to everything. 

All three models (document, relational, and graph) are widely used today, and each is good in its respective domain. One model can be emulated in terms of another model—for example, graph data can be represented in a relational database—but the result is often awkward. That’s why we have different systems for different purposes, not a single one-size-fits-all solution. 

One thing that document and graph databases have in common is that they typically don’t enforce a schema for the data they store, which can make it easier to adapt applications to changing requirements. However, your application most likely still assumes that data has a certain structure; it’s just a question of whether the schema is explicit (enforced on write) or implicit (handled on read).

### **What are the types of query languages for data? where does this type fit in/ perform best?**
### Declarative vs Imperative queries
- An **imperative language** tells the computer to perform certain operations in a certain order.
- In a **declarative query language**, like SQL or relational algebra, you just specify the pattern of the data you want—what conditions the results must meet, and how you want the data to be transformed (e.g., sorted, grouped, and aggregated)—but not how to achieve that goal. It is up to the database system’s query optimizer to decide which indexes and which join methods to use, and in which order to execute various parts of the query.

Imperative code is very hard to parallelize across multiple cores and multiple machines, because it specifies instructions that must be performed in a particular order. 

Declarative languages have a better chance of getting faster in parallel execution because they specify only the pattern of the results, not the algorithm that is used to determine the results.


**Bonus:**
MapReduce is a programming model for processing large amounts of data in bulk across many machines, popularized by Google [33].

MapReduce is neither a declarative query language nor a fully imperative query API, but somewhere in between: the logic of the query is expressed with snippets of code, which are called repeatedly by the processing framework. It is based on the map (also known as collect) and reduce (also known as fold or inject) functions that exist in many functional programming languages.


### **What is graph-like data models? What is the use case?**

If many-to-many relationships are very common in your data, the relational model can handle simple cases of many-to-many relationships, but as the connections within your data become more complex, it becomes more natural to start modeling your data as a graph. A graph consists of two kinds of objects: vertices (also known as nodes or entities) and edges (also known as relationships or arcs). Many kinds of data can be modeled as a graph.

Typical examples include:
- Social graphs 
	- Vertices are people, and edges indicate which people know each other.
- The web graph 
	- Vertices are web pages, and edges indicate HTML links to other pages. 
- Road or rail networks
	- Vertices are junctions, and edges represent the roads or railway lines between them.

But if we put graph data in a relational structure, can we also query it using SQL? The answer is yes, but with some difficulty. In a relational database, you usually know in advance which joins you need in your query. In a graph query, you may need to traverse a variable number of edges before you find the vertex you’re looking for—that is, the number of joins is not fixed in advance.


**Triple-Stores and SPARQL**
In a triple-store, all information is stored in the form of very simple three-part statements: (subject, predicate, object). For example, in the triple (Jim, likes, bananas), Jim is the subject, likes is the predicate (verb), and bananas is the object.

**Property Graphs**
In the property graph model, each vertex consists of: A unique identifier A set of outgoing edges A set of incoming edges A collection of properties (key-value pairs) Each edge consists of: A unique identifier The vertex at which the edge starts (the tail vertex) The vertex at which the edge ends (the head vertex) A label to describe the kind of relationship between the two vertices A collection of properties (key-value pairs) You can think of a graph store as consisting of two relational tables, one for vertices and one for edges,

## Chapter 3 - Storage & Retrieval

### **What Data Structures are there, what are their characteristics?**
The world simplest db is a key-value storage example in book. In order to efficiently find the value for a particular key in the database, we need a different data structure: an index. An index is an additional structure that is derived from the primary data. Many databases allow you to add and remove indexes, and this doesn’t affect the contents of the database; it only affects the performance of queries

An important trade-off in storage systems: well-chosen indexes speed up read queries, but every index slows down writes.

#### Hash Indexes
Then the simplest possible indexing strategy is this: keep an in-memory hash map where every key is mapped to a byte offset in the data file—the location at which the value can be found.

The benefit is to not use a arbitrary string, rather the number to indicate position on where you can find the value. So that means if me and a friend build similar programs the same key will hash to the same value.

**What they are:** Hash maps implement key-value lookup using arrays and hash functions. Hash the key → get array index → access value directly at that position. 

**Why arrays matter:** Arrays enable O(1) lookup via direct memory addressing (`base + index × size`). Strings/keys don't indicate position—numbers do.

**The hash function's role:** Translates arbitrary keys into array indices deterministically (same key → always same index) but non-sequentially (similar keys → scattered positions to avoid clustering).

**Database hash indexes (Kleppmann Ch3):** In-memory hash map stores `key → disk_byte_offset` mappings. Append-only writes to disk, update offset in RAM. On read: hash key → get offset from RAM → single disk seek.

**The critical constraint:** Entire key directory must fit in memory. Works for bounded key sets (URL shorteners, sessions) where key cardinality is limited even if updates are frequent. Doesn't scale to unbounded unique keys.

**Performance tradeoff:** Memory (storing the index) for speed (O(1) lookup vs O(n) file scan). Alternative structures (B-trees, SSTables) optimize different tradeoffs.

#### Storage Strategy

**Log-structured** (append-only, immutable) **Append-only:** New data is always written to the end of a file/log. You never seek to the middle to insert. Like writing in a notebook—you fill pages forward, never flipping back to edit earlier pages.

**Immutable:** Once data is written, it's never changed. Updates don't overwrite the old value; they append a new version with the same key. Deletes append a tombstone marker rather than erasing the original.

- Bitcask
- SSTables/LSM-trees

**Update-in-place** (overwrite fixed locations) When data changes, you find where the old version lives on disk and overwrite it with the new version at that same location. The old value is destroyed.

**Fixed locations:** Data lives in predetermined positions (pages/blocks) on disk. B-trees organize data into fixed-size pages (typically 4KB-16KB). When you update a key, you find which page contains it, read that page into memory, modify it, and write the modified page back to the same disk location.

- B-trees

#### Index Structure
This determines _how you locate data_ within your storage strategy:

**Hash-based** (key → offset/location)

- Bitcask: hashmap in memory → offset in append-only log on disk
- No range queries, only point lookups

**Sorted/Tree-based** (keys in order)

- SSTables: sorted key-value pairs on disk
- B-trees: sorted keys in tree of pages
- Enables range queries

## How they combine:

|Storage Strategy|Index Structure|Example|Range Queries?|
|---|---|---|---|
|Log-structured|Hash|Bitcask|No|
|Log-structured|Sorted|LSM-tree (SSTables)|Yes|
|Update-in-place|Sorted|B-tree|Yes|
|Update-in-place|Hash|(not common—loses B-tree's ordering benefits)|No|

**The key insight:** Log vs update-in-place answers "how do we write?". Hash vs sorted answers "how do we find?". They're orthogonal concerns that combine differently.

SSTables specifically = log-structured storage + sorted index structure.

#### Other indexing structures

**Heap file approach:**
```
Index: [key_1 → heap_offset_500]
       [key_2 → heap_offset_750]

Heap file: [offset_500: full_row_data_1]
           [offset_750: full_row_data_2]
```

**Clustered index:**
```
Index: [key_1 → full_row_data_1 embedded here]
       [key_2 → full_row_data_2 embedded here]
```
(No separate heap file)


**Covering index:**
```
Index: [key_1 → {col_A, col_B} embedded, heap_offset_500 for rest]
       [key_2 → {col_A, col_B} embedded, heap_offset_750 for rest]

Heap file: [offset_500: {col_C, col_D, col_E...}]
           [offset_750: {col_C, col_D, col_E...}]
```

### **What problem does data warehousing solve?**
It helps solve the problem of doing analytic queries against a database that is optimized for other activities such as handling a large amount of writes. Storing important data in a data warehouse means that business analysts can query with heavier queries and it doesn't affect the other database that heavy queries are running. Also usually frequent insert/updates to this data isn't as critical as we look at the big picture.

It also solves the **schema integration problem** - consolidating data from multiple sources (transactional DBs, APIs, event streams) into unified schemas for cross-system analysis. Warehouses retain historical data that transactional systems would purge, and use fundamentally different storage/indexing strategies (columnar storage, star schemas) optimized for read-heavy analytical access patterns rather than individual transactions.
### **What is column oriented storage, when is it useful and what does it solve?**
Column-oriented storage solves the problem of reading unnecessary data when running analytical queries that only need a subset of columns across many rows. 

In row-oriented storage, reading `revenue` from 100M transactions means loading entire rows (customer_id, timestamp, product_id, revenue, shipping_address, etc.) into memory even though you only need one field. This wastes I/O bandwidth and memory.

Column storage keeps each column's values contiguous on disk, so reading `SUM(revenue)` only touches the revenue column - massive performance gains for analytical queries that aggregate specific metrics across large datasets. Also enables better compression since similar data types cluster together (all timestamps, all currency values, etc.).

The tradeoff: writes become expensive because inserting one row means updating multiple separate column files. This is why it pairs with batch loading patterns in warehouses rather than transactional workloads.

## Chapter 4 - Encoding

When using both SQL + JSON: SQL is almost always source of truth
## **Why is Encoding an important subject in the book?**

Whenever you want to send some data to another process with which you don’t share memory—for example, whenever you want to send data over the network or write it to a file—you need to encode it as a sequence of bytes.

Programs usually work with data in (at least) two different representations:
- In memory, data is kept in objects, structs, lists, arrays, hash tables, trees, and so on. These data structures are optimized for efficient access and manipulation by the CPU (typically using pointers).

- When you want to write data to a file or send it over the network, you have to encode it as some kind of self-contained sequence of bytes (for example, a JSON document). Since a pointer wouldn’t make sense to any other process, this sequence-of-bytes representation looks quite different from the data structures that are normally used in memory.

Thus, we need some kind of translation between the two representations. 

The translation from the in-memory representation to a byte sequence is called encoding (also known as serialization or marshalling), and the reverse is called decoding (parsing, deserialization, unmarshalling).

### **What different dataflows are important and what are their characteristics?**
#### DATABASE dataflow
- One process writes, another reads (often same app, different time)
- Schema evolution challenge: newer code writes, older code must read (forward compatibility critical)
- Backward compatibility also needed: rolling updates mean mixed versions
- Key characteristic: **asynchronous, same system boundary**
#### SERVICE CALL DATAFLOW
(Dataflow Through Services (REST/RPC))
- Client sends request, server responds
- Schema evolution: clients often deploy slowly (mobile apps), servers update frequently
- Backward compatibility critical (new server, old clients)
- Key characteristic: **synchronous request/response, crosses system boundary**
#### Asynchronous DATAFLOW
(Dataflow Through Message Queues/Streams)
- Producer publishes message, consumer(s) process asynchronously
- Schema evolution: consumers and producers deploy independently
- Both directions of compatibility needed
- Key characteristic: **asynchronous, decoupled timing, potentially multiple consumers**


**The Distinguishing Dimensions:**
- **Timing**: synchronous (RPC) vs asynchronous (database, queues)
- **Cardinality**: one-to-one (database, RPC) vs one-to-many (message queues)
- **Coupling**: tight (RPC - consumer must be available) vs loose (queues - producer doesn't know consumers exist)

**Why This Matters for Encoding:** Each pattern has different **compatibility pressure points**. RPC needs rock-solid backward compatibility (old clients). Databases need forward compatibility (old code reads new data). Queues need both because you don't control consumer deployment timing.

## How and why are binary schemas used?

**Why Binary Matters:**
- **Compactness**: 2-10x smaller than JSON (no field names repeated)
- **Speed**: Faster parsing (no string parsing, direct memory layout)
- **Type safety**: Schema enforced at encode/decode time
- **Evolvability**: Explicit rules for adding/removing fields

Binary formats encode data compactly by removing field names from the wire - they're in a separate schema instead.

**The Three Major Approaches:**

**1. Thrift & Protocol Buffers (Protobuf)**

- **Tag-based encoding**: Each field has a number (tag) in the schema
- Wire format: `[field_tag][type][value]`
- Schema evolution: Add new fields with new tag numbers
- **Field tags are the contract** - never reuse a tag number
- Forward compatible: old code ignores unknown tags
- Backward compatible: new fields must be optional or have defaults

**2. Avro**

- **No field tags** - relies on schema ordering
- Writer schema (what encoder used) + Reader schema (what decoder expects)
- Schema resolution: matches fields by name, handles differences
- **Schema must travel with data** (embedded or in registry)
- Key difference: easier to generate schemas dynamically (useful for database dumps)
- Schema evolution: can add/remove fields, but reader/writer must be compatible

## Chapter 5 - Replication

**Three reasons:**
1. **Availability/fault tolerance** - System keeps working when nodes fail. If one machine dies, others have the data.
2. **Read scalability** - Distribute read traffic across multiple nodes. One leader can't handle 10,000 reads/sec, but 10 followers can handle 1,000 each.
3. **Latency** - Place data geographically close to users. EU users read from EU replica, US users from US replica - faster than crossing oceans.

**Trade-off:** Keeping copies synchronized creates all the complexity this chapter explores (lag, conflicts, consistency). You only replicate when these benefits outweigh the coordination cost.

### **What are the three replication topologies (structures) and what does each optimize for?
**Single-leader:** Optimizes for consistency and simplicity. One node accepts writes → no conflicts possible. Trade-off: write availability (leader failure blocks writes) and write scalability (bottleneck).

**Multi-leader:** Optimizes for write availability and geographic latency. Multiple nodes accept writes → system continues during network partitions. Trade-off: conflict resolution complexity.

**Leaderless:** Optimizes for high availability and fault tolerance. Any replica accepts writes, quorum-based coordination. Trade-off: weak consistency guarantees, application must handle conflicts.

### **What does async replication optimize for, and what consistency violations does replication lag cause in single-leader systems?**

**What async replication optimizes for:** Read scalability and availability. You can add unlimited followers to handle read traffic, and the system keeps working even if followers are slow/down. The leader doesn't wait for acknowledgment before confirming writes to the client.

**What it sacrifices (consistency violations):**

Three timing problems emerge when followers lag behind the leader:

1. **Read-after-write violation** - You write something, immediately read it back, but hit a stale follower that hasn't caught up yet. You don't see your own write.

2. **Monotonic read violation** - You read from follower A (up-to-date), then read from follower B (lagging). Time appears to go backward - you see older data on the second read.

3. **Consistent prefix violation** - Causally-related writes arrive out of order. Like seeing a reply before the original message, or an effect before its cause.

**The core trade-off:** Async replication = "eventual consistency" - all replicas will eventually agree, but there's a window where clients see inconsistent views. You trade immediate correctness for scalability/performance.

### **What are the unique conflict/concurrency problems in multi-leader and leaderless replication, and how are they handled?**

**Multi-leader problem:** Two leaders accept conflicting writes to the same data simultaneously (e.g., user updates profile in US datacenter, same user updates it in EU datacenter at same time). Unlike single-leader, there's no authority to serialize writes - both are valid from each datacenter's perspective.

**How handled:** System must detect conflicts and resolve them. Options: last-write-wins (arbitrary, loses data), application code decides, or merge both writes (CRDTs). No perfect solution - you're choosing which write to discard or how to combine them.

**Leaderless problem:** Any node accepts writes, no leader to coordinate. When writes happen concurrently across nodes, the system can't tell which came "first" - causality is ambiguous. Even with quorums (e.g., write to 2/3 nodes, read from 2/3), you can read stale data if nodes are out of sync.

**How handled:** Version vectors/version clocks track causality - system detects when writes are truly concurrent vs. one superseding another. Quorums provide probabilistic consistency (usually works, but edge cases exist). Application might still need to resolve conflicts when concurrent writes are detected.

**Core insight:** When multiple nodes accept writes, you lose the single source of truth. You're trading write availability for conflict complexity.

## Chapter 6 - Partitioning

### **How do you split data across machines without creating bottlenecks?**

Two main strategies exist, each with distinct tradeoffs:

- **Key range partitioning** divides data by sorted key ranges—like encyclopedia volumes where A-B is one volume, C-D another. This allows efficient range queries (e.g., "find all sensor readings from March") since related data sits together. The danger: if keys aren't evenly distributed, or if access patterns favor certain ranges (like writing only today's timestamps to the current partition), you get hot spots where one machine does all the work while others sit idle.

- **Hash partitioning** applies a hash function to keys and assigns ranges of hash values to partitions. This scrambles the data to distribute load more evenly, preventing hot spots from natural key clustering. The cost: you lose the ability to do efficient range queries since related keys now scatter across all partitions.

Some databases (like Cassandra) use a hybrid: hash the first part of a compound key for distribution, then sort by remaining parts within each partition—getting both even distribution and limited range query capability.

**Secondary indexes** complicate both approaches. With **document-partitioned indexes** (local indexes), each partition maintains its own index covering only its data—writes touch one partition, but reads must query all partitions (scatter/gather). With **term-partitioned indexes** (global indexes), the index itself is partitioned—reads query specific partitions efficiently, but writes must update multiple index partitions.

---

### **How do you add or remove machines without massive data movement?**

The process is called **rebalancing**. Three approaches have evolved:

**Fixed number of partitions**: Create far more partitions than nodes upfront (e.g., 1,000 partitions across 10 nodes). When adding a node, it steals some partitions from existing nodes. Only partition-to-node assignment changes—keys stay in their partitions. Simple operationally, but choosing the right initial partition count is difficult: too few limits growth, too many adds overhead.

**Dynamic partitioning**: Partitions split when they grow too large (like B-tree nodes) and merge when they shrink. Adapts naturally to data volume. The challenge: an empty database starts with one partition—all writes hit one node initially until the first split happens. Pre-splitting can mitigate this if you know key distribution upfront.

**Partitions proportional to nodes**: Each node gets a fixed number of partitions (e.g., 256). When a node joins, it splits random existing partitions and takes half of each. Partition size stays relatively stable as both nodes and data grow together.

All approaches face the same risk: **automatic rebalancing during node overload can trigger cascading failures**. A struggling node appears dead, rebalancing moves load away, which further overloads the network and other nodes. Many systems require human approval for rebalancing operations.

---

### **How does a client know which machine has the data it needs?**

This is the **request routing problem**—solving it requires all participants to agree on partition assignments as they change. Three architectural patterns exist:

**Contact any node**: Clients hit any node (via load balancer). If that node has the data, it responds directly; otherwise it forwards to the correct node. Cassandra and Riak use this approach with a **gossip protocol** where nodes share state changes among themselves—more node complexity, no external dependencies.

**Routing tier**: All requests go through a dedicated routing service that forwards them to the right node without handling data itself. The router must track partition assignments.

**Partition-aware clients**: Clients track partition assignments themselves and connect directly to the correct node—eliminates intermediaries but pushes complexity to clients.

The fundamental challenge: keeping the routing information synchronized as assignments change. Many systems use **ZooKeeper** (or similar coordination services) as the source of truth. Nodes register themselves in ZooKeeper, which maintains the authoritative partition-to-node mapping. Routers and clients subscribe to ZooKeeper for updates when assignments change.

For simpler cases where assignments change rarely, DNS can handle node discovery, as IP addresses change less frequently than partition assignments.