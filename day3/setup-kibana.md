### Installation 

```
sudo apt install kibana
cd /etc/kibana/
  554  ls
  555  cat kibana.yml 
  556  vim kibana.yml 

```
changing config file 

<img src="kb1.png">

### start service 

```
 557  systemctl start kibana.service 
  558  systemctl status kibana.service 
  559  netstat -nlpt
```

### reset password of kibana_system 

```
 /usr/share/elasticsearch/bin/elasticsearch-reset-password  -u kibana_system
```
### kibana.yaml file 

```
server.port: 5601
server.host: "0.0.0.0"
elasticsearch.username: "kibana_system"
elasticsearch.password: "you get from reset "
elasticsearch.ssl.verificationMode: none

```

### restart service 

```
systemctl restart kibana

```

### openUI  http://yourip:5601 

username & password (elastic , yourpassword)

