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
