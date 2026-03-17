### info about 

<img src="fileb.png">

### setup 

```
sudo apt install filebeat

===>
filebeat version 
filebeat version 8.19.12 (amd64), libbeat 8.19.12 [fcd700e2e9cd1b3f078809e74d5a989243c0c0b0 built 2026-02-24 08:42:12 +0000 UTC] (FIPS-distribution: false)

===>


filebeat version 
filebeat version 8.19.12 (amd64), libbeat 8.19.12 [fcd700e2e9cd1b3f078809e74d5a989243c0c0b0 built 2026-02-24 08:42:12 +0000 UTC] (FIPS-distribution: false)
root@ip-172-31-7-140:~# 
root@ip-172-31-7-140:~# 
root@ip-172-31-7-140:~# 
root@ip-172-31-7-140:~# cd /etc/filebeat/
root@ip-172-31-7-140:/etc/filebeat# ls
fields.yml  filebeat.reference.yml  filebeat.yml  modules.d
root@ip-172-31-7-140:/etc/filebeat# ls modules.d/
activemq.yml.disabled     cyberarkpas.yml.disabled       juniper.yml.disabled          netflow.yml.disabled     salesforce.yml.disabled
apache.yml.disabled       elasticsearch.yml.disabled     kafka.yml.disabled            nginx.yml.disabled       santa.yml.disabled
auditd.yml.disabled       envoyproxy.yml.disabled        kibana.yml.disabled           o365.yml.disabled        snyk.yml.disabled
aws.yml.disabled          fortinet.yml.disabled       

```

### listing and enabling module to read logs by filebeat 

```
root@ip-172-31-7-140:/etc/filebeat# filebeat  modules enable apache
Enabled apache
root@ip-172-31-7-140:/etc/filebeat# filebeat  modules list
Enabled:
apache

Disabled:
activemq
auditd
aws
awsfargate
azure

```
### /etc/filebeat/modules.d/apache.yml 

```
oot@ip-172-31-7-140:/etc/filebeat/modules.d# cat apache.yml 
# Module: apache
# Docs: https://www.elastic.co/guide/en/beats/filebeat/8.19/filebeat-module-apache.html

- module: apache
  # Access logs
  access:
    enabled: true

    # Set custom paths for the log files. If left empty,
    # Filebeat will choose the paths depending on your OS.
    var.paths:
     - /var/log/apache2/access.log

  # Error logs
  error:
    enabled: true

    # Set custom paths for the log files. If left empty,
    # Filebeat will choose the paths depending on your OS.
    var.paths:
     - /var/log/apache2/error.log

```