# Multi-node Elasticsearch Cluster with Docker Compose

## 1️⃣ Prerequisites

Ensure these are installed:

- **Docker**  
    Check Docker:  
    ```sh
    docker --version
    ```

- **Docker Compose**  
    Check Docker Compose:  
    ```sh
    docker compose version
    ```
    If Compose is missing:  
    ```sh
    sudo apt install docker-compose-plugin
    ```

---

## 2️⃣ Create Project Directory

Create a directory for the cluster:

```sh
mkdir elastic-cluster
cd elastic-cluster
```

---

## 3️⃣ Create Docker Compose File

Create the file:

```sh
vim docker-compose.yml
```

Paste this configuration:

```yaml
version: "3"

services:
    es01:
        image: docker.elastic.co/elasticsearch/elasticsearch:8.11.0
        container_name: es01
        environment:
            - node.name=es01
            - cluster.name=demo-cluster
            - discovery.seed_hosts=es02,es03
            - cluster.initial_master_nodes=es01,es02,es03
            - bootstrap.memory_lock=true
            - xpack.security.enabled=false
            - ES_JAVA_OPTS=-Xms1g -Xmx1g
        volumes:
            - esdata01:/usr/share/elasticsearch/data
        ports:
            - 9200:9200
        networks:
            - elastic

    es02:
        image: docker.elastic.co/elasticsearch/elasticsearch:8.11.0
        container_name: es02
        environment:
            - node.name=es02
            - cluster.name=demo-cluster
            - discovery.seed_hosts=es01,es03
            - cluster.initial_master_nodes=es01,es02,es03
            - bootstrap.memory_lock=true
            - xpack.security.enabled=false
            - ES_JAVA_OPTS=-Xms1g -Xmx1g
        volumes:
            - esdata02:/usr/share/elasticsearch/data
        networks:
            - elastic

    es03:
        image: docker.elastic.co/elasticsearch/elasticsearch:8.11.0
        container_name: es03
        environment:
            - node.name=es03
            - cluster.name=demo-cluster
            - discovery.seed_hosts=es01,es02
            - cluster.initial_master_nodes=es01,es02,es03
            - bootstrap.memory_lock=true
            - xpack.security.enabled=false
            - ES_JAVA_OPTS=-Xms1g -Xmx1g
        volumes:
            - esdata03:/usr/share/elasticsearch/data
        networks:
            - elastic

volumes:
    esdata01:
    esdata02:
    esdata03:

networks:
    elastic:
```

---

## 4️⃣ Understand Important Settings

These environment variables replace `elasticsearch.yml`:

- **Node identity:**  
    `node.name=es01`

- **Cluster name:**  
    `cluster.name=demo-cluster`

- **Node discovery:**  
    `discovery.seed_hosts=es02,es03`

- **Initial master election:**  
    `cluster.initial_master_nodes=es01,es02,es03`

- **Disable security (for lab):**  
    `xpack.security.enabled=false`

- **Heap configuration:**  
    `ES_JAVA_OPTS=-Xms1g -Xmx1g`

---

## 5️⃣ Start the Cluster

Run:

```sh
docker compose up -d
```

Docker will:

- Pull images
- Create network
- Start 3 containers
- Form cluster

---

## 6️⃣ Verify Containers

```sh
docker ps
```

You should see:

- es01
- es02
- es03

---

## 7️⃣ Check Elasticsearch Cluster

Run:

```sh
curl http://localhost:9200/_cluster/health?pretty
```

Expected result:

```json
{
    "cluster_name" : "demo-cluster",
    "status" : "green",
    "number_of_nodes" : 3
}
```

---

## 8️⃣ Check Nodes in Cluster

```sh
curl http://localhost:9200/_cat/nodes?v
```

Example output:

```
ip        heap.percent ram.percent node.role name
172.18... 25           60          dimr      es01
172.18... 30           61          dimr      es02
172.18... 20           59          dimr      es03
```

Now you have 3 nodes running.

---

## 9️⃣ Check Shard Allocation

```sh
curl http://localhost:9200/_cat/shards?v
```

When indices exist, shards will be distributed across nodes.

---

## 🔟 Create Test Index

Create index with shards and replicas:

```sh
curl -X PUT localhost:9200/test-index -H "Content-Type: application/json" -d'
{
    "settings": {
        "number_of_shards": 3,
        "number_of_replicas": 1
    }
}'
```

Now check shards:

```sh
curl localhost:9200/_cat/shards?v
```

You will see shards distributed across nodes.

---

## 1️⃣1️⃣ Stop Cluster

```sh
docker compose down
```

This stops containers but data persists because of volumes.

---

## 1️⃣2️⃣ Remove Everything

If you want a full reset:

```sh
docker compose down -v
```

This removes volumes and data.