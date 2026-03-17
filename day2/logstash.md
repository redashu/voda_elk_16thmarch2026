### intro 

<img src="intro.png">

# What is Logstash?

Logstash is a server-side data processing pipeline that ingests, transforms, and sends data to multiple destinations.

Think of it as: **Logstash = Data Transformation Engine**

## Why Logstash Exists (Problem It Solves)

You already saw:

```
Apache → Filebeat → Elasticsearch
```

This works, but:
- Logs are raw text
- No enrichment (geo-IP, user info, etc.)
- No transformation
- No routing logic

Now imagine multiple sources:
- Apache logs
- DB logs
- Firewall logs
- Application logs

Each has different formats and structures.

👉 You need a central processor. That is Logstash.

## Logstash Core Architecture

Logstash works in pipeline stages:

```
INPUT → FILTER → OUTPUT
```

### INPUT

Where data comes from:
- Filebeat (beats input)
- Kafka
- HTTP
- File
- Syslog

### FILTER (Most Important Part)

This is where real processing happens. You can:
- Parse logs (grok)
- Convert fields
- Add/remove fields
- Enrich data (geoip, user info)
- Transform timestamps

Example transformation:

```
"192.168.1.10 - - [16/Mar/2026] GET /home 200"

becomes:

{
    "ip": "192.168.1.10",
    "method": "GET",
    "url": "/home",
    "status": 200
}
```

### OUTPUT

Where data goes:
- Elasticsearch
- Kafka
- File
- S3
- Multiple outputs

## Where Logstash Fits (With Beats)

**Without Logstash:**
```
Apache → Filebeat → Elasticsearch
```
- ✔ Simple
- ❌ Limited processing

**With Logstash:**
```
Apache → Filebeat → Logstash → Elasticsearch
```
- ✔ Full control
- ✔ Transformation
- ✔ Enrichment
- ✔ Routing

## What is Filebeat?

Filebeat is a lightweight log shipper installed on servers that:
- Reads log files
- Handles rotation
- Batches logs
- Sends logs

But has limitations:
- ❌ No heavy processing
- ❌ Limited parsing

## What is Fluentd?

Fluentd is similar to Logstash but:
- More lightweight than Logstash
- Written in Ruby
- Highly flexible
- Popular in Kubernetes

## Comparison (Very Important)

| Feature | Filebeat | Logstash | Fluentd |
|---------|----------|----------|---------|
| Type | Shipper | Processor | Collector + Processor |
| Resource | Very low | High | Medium |
| Language | Go | Java | Ruby |
| Parsing | Limited | Very powerful | Powerful |
| Use case | Collect logs | Transform logs | Flexible pipelines |

## Real Industry Architecture

Modern production pipelines look like:

```
Servers
     │
     ▼
Filebeat
     │
     ▼
Logstash (cluster)
     │
     ▼
Elasticsearch
     │
     ▼
Kibana
```

## Modern Alternative (Important Insight)

In Elasticsearch 8.x, Logstash is sometimes replaced by:

```
Filebeat → Ingest Pipeline → Elasticsearch
```

Benefits:
- Ingest pipelines can parse logs
- Lower infrastructure cost
- Easier setup

You are already using this (Apache module).

## When to Use Logstash

Use Logstash when:
- Logs are unstructured
- Complex parsing required
- Multiple sources need merging
- Data enrichment required
- Routing logic needed
- Sending data to multiple systems

## When NOT to Use Logstash

Skip Logstash when:
- Logs already structured (JSON)
- Simple pipelines
- Low complexity
- Filebeat modules are sufficient

## Conceptual Difference (Very Clear)

- **Filebeat** = collects logs
- **Logstash** = processes logs
- **Fluentd** = alternative processor/collector

## Your Current Setup

You currently have:

```
Apache → Filebeat → Elasticsearch (Data Stream)
```

This uses:
- Filebeat module
- Elasticsearch ingest pipeline
- Data streams

👉 This is already a modern pipeline without Logstash