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