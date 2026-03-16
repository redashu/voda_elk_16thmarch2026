# Elastic Single Node Setup

## Login with Ubuntu Linux Shell

### Activating Repository

```sh
sudo apt update
sudo apt upgrade
```

### Adding GPG Key and Elasticsearch Repository

```sh
wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo gpg --dearmor -o /usr/share/keyrings/elasticsearch-keyring.gpg

echo "deb [signed-by=/usr/share/keyrings/elasticsearch-keyring.gpg] https://artifacts.elastic.co/packages/8.x/apt stable main" | sudo tee /etc/apt/sources.list.d/elastic-8.x.list
```

```sh
sudo apt update
```

### Installing Elasticsearch

```sh
sudo apt install elasticsearch
# ===> aRq=W8XML3_z5I2XMQT9
```

### After Installation: Things to Check

```sh
sudo -i
cd /etc/elasticsearch/
ls
# certs  elasticsearch.keystore  jvm.options  log4j2.properties  roles.yml  users_roles
# elasticsearch-plugins.example.yml  elasticsearch.yml  jvm.options.d  role_mapping.yml  users
```

### Taking Backup of Config File

```sh
cp -v elasticsearch.yml elasticsearch.yml.backup
```

### Starting with Default Settings

```sh
systemctl daemon-reload
systemctl start elasticsearch
systemctl enable elasticsearch
# Created symlink /etc/systemd/system/multi-user.target.wants/elasticsearch.service → /usr/lib/systemd/system/elasticsearch.service.
systemctl status elasticsearch
# ● elasticsearch.service - Elasticsearch
#      Loaded: loaded (/usr/lib/systemd/system/elasticsearch.service; enabled; preset: enabled)
#      Active: active (running) since Mon 2026-03-16 07:03:57 UTC; 16s ago
#        Docs: https://www.elastic.co
#    Main PID: 12155 (java)
#       Tasks: 110 (limit: 9496)
#      Memory: 4.3G (peak: 4.3G)
#         CPU: 55.094s
#      CGroup: /system.slice/elasticsearch.service
```

## using curl to interact with elasticsearch 

```
url  --cacert  certs/http_ca.crt   -u elastic  https://localhost:9200 
Enter host password for user 'elastic':
{
  "name" : "ip-172-31-7-140",
  "cluster_name" : "elasticsearch",
  "cluster_uuid" : "UJuS29qsQFOsMkjZFR670w",
  "version" : {
    "number" : "8.19.12",
    "build_flavor" : "default",
    "build_type" : "deb",
    "build_hash" : "840cd2a58b052d1632219ee0b8dcc0f364226287",
    "build_date" : "2026-02-23T23:08:40.713020893Z",
    "build_snapshot" : false,
    "lucene_version" : "9.12.2",
    "minimum_wire_compatibility_version" : "7.17.0",
    "minimum_index_compatibility_version" : "7.0.0"
  },
  "tagline" : "You Know, for Search"
}

```
### some more tools for advanced administration and troubleshooting 

```
ls /usr/share/elasticsearch/
NOTICE.txt  README.asciidoc  bin  jdk  lib  modules  plugins
root@ip-172-31-7-140:/etc/elasticsearch# ls /usr/share/elasticsearch/bin/
elasticsearch                          elasticsearch-env            elasticsearch-reconfigure-node  elasticsearch-sql-cli
elasticsearch-certgen                  elasticsearch-env-from-file  elasticsearch-reset-password    elasticsearch-sql-cli-8.19.12.jar
elasticsearch-certutil                 elasticsearch-geoip          elasticsearch-saml-metadata     elasticsearch-syskeygen
elasticsearch-cli                      elasticsearch-keystore       elasticsearch-service-tokens    elasticsearch-users
elasticsearch-create-enrollment-token  elasticsearch-node           elasticsearch-setup-passwords   systemd-entrypoint
elasticsearch-croneval                 elasticsearch-plugin         elasticsearch-shard

```

### resetting password of elastic user 

```
/usr/share/elasticsearch/bin/elasticsearch-reset-password  -u elastic -i 
This tool will reset the password of the [elastic] user.
You will be prompted to enter the password.
Please confirm that you would like to continue [y/N]y


Enter password for [elastic]: 
Re-enter password for [elastic]: 
Password for the [elastic] user successfully reset.

```

## CUrl to interact with cluster 

```
 curl  --cacert  /etc/elasticsearch/certs/http_ca.crt   -u elastic:Redhat@12345 https://localhost:9200

```
### using linux alias to check 

```
alias elk-curl='curl  --cacert  /etc/elasticsearch/certs/http_ca.crt   -u elastic:Redhat@12345'

===>
elk-curl  https://localhost:9200/_cluster/health?pretty 

```
### checking _cat endpoint of elasticsearch 

```
lk-curl  https://localhost:9200/_cat/
=^.^=
/_cat/allocation
/_cat/shards
/_cat/shards/{index}
/_cat/master
/_cat/nodes
/_cat/tasks
/_cat/indices
/_cat/indices/{index}
/_cat/segments
/_cat/segments/{index}
/_cat/count
/_cat/count/{index}
/_cat/recovery
/_cat/recovery/{index}
/_cat/health
/_cat/pending_tasks
/_cat/aliases
/_cat/aliases/{alias}
/_cat/thread_pool
/_cat/thread_pool/{thread_pools}
/_cat/plugins
/_cat/fielddata
/_cat/fielddata/{fields}
/_cat/nodeattrs
/_cat/repositories
/_cat/snapshots/{repository}
/_cat/templates
/_cat/component_templates
/_cat/ml/anomaly_detectors
/_cat/ml/anomaly_detectors/{job_id}
/_cat/ml/datafeeds
/_cat/ml/datafeeds/{datafeed_id}
/_cat/ml/trained_models
/_cat/ml/trained_models/{model_id}
/_cat/ml/data_frame/analytics
/_cat/ml/data_frame/analytics/{id}
/_cat/transforms
/_cat/transforms/{transform_id}
root@ip-172-31-7-140:/etc/elasticsearch# elk-curl  https://localhost:9200/_cat/nodes
127.0.0.1 47 91 0 0.00 0.00 0.00 cdfhilmrstw * ip-172-31-7-140
root@ip-172-31-7-140:/etc/elasticsearch# elk-curl  https://localhost:9200/_cat/nodes?v
ip        heap.percent ram.percent cpu load_1m load_5m load_15m node.role   master name
127.0.0.1           47          91   1    0.07    0.02     0.00 cdfhilmrstw *      ip-172-31-7-140

```

### creating index 

```
curl --cacert /etc/elasticsearch/certs/http_ca.crt -u elastic:Redhat@12345 -X PUT https://localhost:9200/myindex \
-H "Content-Type: application/json" \
-d '{
    "settings": {
        "number_of_shards": 1,
        "number_of_replicas": 0
    }
}'

```

### listing index 

```
 elk-curl  -X GET https://localhost:9200/_cat/indices?v
```

### understanding output of  list index 

### Detailed Column Explanation

| Column | Value | Explanation |
| --- | --- | --- |
| **health** | green | All shards working properly. Green = Primary and replica shards allocated (best state). Yellow = Primary allocated, replicas missing. Red = Primary shards unallocated. |
| **status** | open | Index is open and available for search/indexing. |
| **index** | myindex | Index name (letters, numbers, underscores, hyphens allowed). |
| **uuid** | cWYa5sIETp6JP1XmwNf9Qg | Unique identifier for tracking in logs/debugging. |
| **pri** | 1 | Number of primary shards (cannot be changed after creation). |
| **rep** | 0 | Replica shards per primary (0 = no copies, risky for production). |
| **docs.count** | 0 | Number of documents (empty index). |
| **docs.deleted** | 0 | Deleted documents (marked for removal during merges). |
| **store.size** | 227b | Total disk space (includes metadata, segments, translog). |
| **pri.store.size** | 227b | Primary shard disk usage only. |
| **dataset.size** | 227b | Actual data size without storage overhead. |


## Inserting docs in index 

```
elk-curl  -X POST https://localhost:9200/ashu-data/_doc/1 -H "Content-Type: application/json" \
-d '{
  "user": "ashu",
  "action": "login",
  "status": "failed"
}'

===>
