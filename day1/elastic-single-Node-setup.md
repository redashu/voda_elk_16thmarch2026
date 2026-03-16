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
