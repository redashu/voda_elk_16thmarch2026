# The Core Concepts - What IS Elasticsearch?

Forget everything you know about databases for a moment. Elasticsearch is a distributed search and analytics engine. Let's break that down:

## 1.1 The Three Core Identities

Elasticsearch is actually three things in one:

### Identity 1: A Search Engine

- Like Google, but for your data
- You type "payment failed ORD-12345" and it finds relevant documents
- It understands relevance scoring (how well does this match?)
- It handles typos, partial matches, synonyms if configured

### Identity 2: A Database

- It stores documents permanently (unless you delete them)
- It can retrieve documents by ID
- It can count, sum, average (analytics)
- But: It's not transactional. No ACID guarantees across documents.

### Identity 3: An Analytics Engine

- "Show me error trends over time"
- "What are the top 10 IP addresses hitting our servers?"
- "Average response time by endpoint"

## 1.2 The Building Blocks (Learn These First)

Let's use a library analogy:

| Concept | Analogy | Real Meaning |
|---------|---------|--------------|
| Cluster | The entire library building | One or more nodes working together |
| Node | A single bookshelf | A single Elasticsearch server instance |
| Index | A specific section (e.g., "Mystery Books") | A collection of related documents (like a database in SQL) |
| Type | (Deprecated) Used to be like book genres within a section | Don't worry about this - it's gone in modern versions |
| Document | A single book | A single JSON object (like a row in SQL) |
| Field | A page in the book | A key-value pair in the JSON |
| Mapping | The card catalog telling you what each field means | Schema definition (which fields are text, numbers, dates) |
| Shard | A split of the mystery section across multiple floors | How Elasticsearch breaks data into pieces for distribution |

## Architecture

![ELK Architecture Diagram](arch-elk.png)

### 1️⃣ High-Level Architecture of Elasticsearch

At the top level, Elasticsearch works in this hierarchy:

```
Cluster → Nodes → Indices → Shards → Segments
```

We will go step by step.

### 2️⃣ Cluster

**What is a Cluster?**

A cluster is a collection of one or more Elasticsearch nodes working together.

All nodes share:
- Same cluster name
- Same cluster state
- Same data view

**Example:** If you deploy Elasticsearch on 3 servers → they form 1 cluster.

**Important:** Even a single Elasticsearch instance is technically a cluster.

#### Single Node Cluster

- 1 node
- Holds all data
- No high availability
- No horizontal scaling

**Good for:**
- Development
- Testing
- Small workloads

#### Multi Node Cluster

- 2+ nodes
- Data distributed across nodes
- Supports horizontal scaling, high availability, fault tolerance, load balancing

### 3️⃣ Node

A node is a single running Elasticsearch instance (JVM process).

Each node has its own heap memory, CPU usage, disk storage, and communicates with other nodes over TCP.

**Types of Nodes:**

| Type | Responsibility |
|------|---|
| Master-Eligible | Cluster state, index creation/deletion, shard allocation |
| Data | Storing data, executing queries, performing aggregations |
| Coordinating | Receiving client requests, distributing searches, merging results |
| Ingest | Preprocessing documents, pipeline transformations, enrichment |

### 4️⃣ Index

In RDBMS: `Database → Tables → Rows`  
In Elasticsearch: `Index → Documents`

An index is a logical namespace that stores related documents. Each index has mappings, settings, and shards.

### 5️⃣ Document

Smallest unit of data, stored in JSON format:

```json
{
    "user": "ashu",
    "action": "login",
    "status": "failed"
}
```

### 6️⃣ Shards (Core Scaling Mechanism)

A shard is a physical partition of an index. Elasticsearch splits an index into smaller pieces for horizontal scalability.

**Why Shards?** They enable parallel search, parallel indexing, and load distribution across nodes.

### 7️⃣ Primary and Replica Shards

Each shard has a primary shard (accepts writes) and optional replica shards (for high availability and read scaling).

### 8️⃣ Segments (Lucene Level)

Each shard contains segments. When you index data, documents go to a memory buffer, then flush to segment files. Segments are immutable.

### 9️⃣ Scatter-Gather Search Model

1. Request hits coordinating node
2. Query sent to all relevant shards
3. Each shard executes locally
4. Results returned to coordinating node
5. Results merged and sorted
6. Final response sent to client

### 🔟 Cluster Health States

- **Green:** All primary + replicas allocated
- **Yellow:** Primaries allocated, replicas missing
- **Red:** Some primaries missing (data unavailable)

### 1️⃣1️⃣ Production Design Rules

- Minimum 3 master-eligible nodes
- Separate data nodes
- Shard sizing: 20-50GB per shard ideal
- Avoid too many small shards