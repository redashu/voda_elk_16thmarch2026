### using python to connect elasticSearch server

### checking required details 

```
oot@ip-172-31-7-140:~# python3 -V
Python 3.12.3
root@ip-172-31-7-140:~# 

====>

apt install python3-pip -y

===> 
apt install python3-venv -y

```

### setting up python env for writing code 

```
oot@ip-172-31-7-140:~# python3 -m venv  ashu-elk 

root@ip-172-31-7-140:~# ls

ashu-elk  elastic-cluster  html-sample-app  snap
root@ip-172-31-7-140:~# 
root@ip-172-31-7-140:~# 

root@ip-172-31-7-140:~# source ashu-elk/bin/activate
(ashu-elk) root@ip-172-31-7-140:~# 


(ashu-elk) root@ip-172-31-7-140:~# pip install elasticsearch==8.*
Collecting elasticsearch
  Downloading elasticsearch-9.3.0-py3-none-any.whl.metadata (9.0 kB)
Collecting anyio (from elasticsearch)
  Downloading anyio-4.12.1-py3-none-any.whl.metadata (4.3 kB)
```

### time to write sample python code 

```
vim test.py
---
from elasticsearch import Elasticsearch

username = 'elastic'
password = 'Redhat@12345' # Value you set in the environment variable

client = Elasticsearch(
    "https://localhost:9200",
     ca_certs="/etc/elasticsearch/certs/http_ca.crt",
     basic_auth=(username, password)
     #verify_certs=False
)

print(client.info())


===> python test.py 
```
