# Elasticsearch: Real-World Information

## 1️⃣ What is Elasticsearch?

Elasticsearch is a distributed search and analytics engine designed to store, search, and analyze large volumes of data very quickly.

### Main Use Cases
- Log analysis
- Full-text search
- Observability
- Security analytics
- Real-time dashboards

## 2️⃣ Who Created Elasticsearch?

- **Creator**: Shay Banon
- **Year**: 2010
- **Origin**: Initially built to improve search capabilities for his wife's recipe website, later evolved into a powerful search engine

## 3️⃣ Company Behind Elasticsearch

**Elastic NV** develops Elasticsearch and created:
- Kibana
- Logstash
- Beats
- Elastic APM

Together known as the **Elastic Stack** (formerly ELK Stack)

## 4️⃣ Written In Which Language?

**Java** - Why?
- Strong memory management
- Mature ecosystem
- JVM-based performance tuning
- Good concurrency support
- Cross-platform compatibility

**Under the hood**: Apache Lucene
- A high-performance search library (also written in Java)
- Provides indexing & search core engine

> **Important**: Elasticsearch is a distributed layer built on top of Lucene.

## 5️⃣ First Release

- **Initial Release**: 2010
- **Version**: 0.4 (early experimental)

**Major milestones**:
- 1.x → Stability phase
- 2.x → Query DSL improvements
- 5.x → Removal of mapping types
- 6.x → Type deprecation
- 7.x → Single-type index
- 8.x → Security enabled by default

## 6️⃣ Current Major Version

As of 2026, Elasticsearch major version is **8.x series**

**Key characteristics**:
- Security enabled by default (TLS + authentication)
- API key support
- Improved cluster coordination
- Better performance
- Runtime fields
- Enhanced vector search support (for AI use cases)

## 7️⃣ License History (Very Important in Industry)

**Originally**: Apache 2.0 (fully open source)

**In 2021**: Elastic changed license to:
- SSPL (Server Side Public License)
- Elastic License

**Consequence**: Amazon forked Elasticsearch 7.10 and created **OpenSearch**

> Today you hear both: Elasticsearch and OpenSearch (same origin, now different products)

## 8️⃣ Architecture Type

Elasticsearch is:
- Distributed
- RESTful
- Document-oriented
- Schema-flexible
- Near real-time search engine

**Technologies used**:
- JSON over HTTP
- REST APIs
- Sharding
- Replication
- Inverted index

## 9️⃣ Data Format Used

Elasticsearch stores data in **JSON documents**

```json
{
    "user": "ashu",
    "action": "login",
    "status": "failed",
    "timestamp": "2026-03-02T10:30:00"
}
```

This is called a "document".

## 🔟 Where It Runs

Elasticsearch runs on:
- Linux (production standard)
- macOS (development)
- Windows
- Docker
- Kubernetes
- Cloud (Elastic Cloud, AWS, Azure, GCP)

It runs as a JVM process.

## 11️⃣ Why It Became Popular

Modern systems needed:
- Real-time log search
- Microservices monitoring
- Full-text search
- Horizontal scalability
- REST-based API access

Traditional RDBMS could not handle this efficiently.

## 12️⃣ Basic Terminology (Foundation Vocabulary)

You must understand these words early:
- Cluster
- Node
- Index
- Document
- Shard
- Replica
- Mapping
- Analyzer
- Query DSL