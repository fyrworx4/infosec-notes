# Elastic Stack Configuration

### Installing Elasticsearch

```bash
wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -
```

```bash
sudo apt-get install apt-transport-https
```

```bash
echo "deb https://artifacts.elastic.co/packages/7.x/apt stable main" | sudo tee -a /etc/apt/sources.list.d/elastic-7.x.list
```

```
sudo apt-get update && sudo apt-get install elasticsearch
```

### Configuring Elasticsearch

```bash
sudo vi /etc/elasticsearch/elasticsearch.yml
```

