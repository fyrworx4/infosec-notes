# Elastic Stack Configuration

This worked on Ubuntu 20.04

### Configuring Elasticsearch

```bash
wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -
```

```bash
sudo apt-get install apt-transport-https
```

```bash
echo "deb https://artifacts.elastic.co/packages/7.x/apt stable main" | sudo tee -a /etc/apt/sources.list.d/elastic-7.x.list
```

```bash
sudo apt-get update && sudo apt-get install elasticsearch
```

```bash
sudo nano /etc/elasticsearch/elasticsearch.yml
```

Uncomment `network.host` and add in `0.0.0.0` to the end of it:

```
/etc/elasticsearch/elasticsearch.yml

# ---------------------------------- Network -----------------------------------
#
# By default Elasticsearch is only accessible on localhost. Set a different
# address here to expose this node on the network:
#
network.host: 0.0.0.0
#
# By default Elasticsearch listens for HTTP traffic on the first free port it
# finds starting at 9200. Set a specific HTTP port here:
#
#http.port: 9200
#
# For more information, consult the network module documentation.
#
```

Scroll down and change `discovery.seed_hosts` and `cluster.initial_master_nodes`:

```
/etc/elasticsearch/elasticsearch.yml

# --------------------------------- Discovery ----------------------------------
#
# Pass an initial list of hosts to perform discovery when this node is started:
# The default list of hosts is ["127.0.0.1", "[::1]"]
#
discovery.seed_hosts: [“127.0.0.1”]
#
# Bootstrap the cluster using an initial set of master-eligible nodes:
#
cluster.initial_master_nodes: ["node-1"]
#
# For more information, consult the discovery and cluster formation module documentation.
#
```

```bash
sudo service elasticsearch start
sudo service elasticsearch status
```

```bash
curl -X GET “IP OF YOUR ELASTIC HOST:9200/?pretty”
```

### Configuring Kibana

```bash
sudo apt-get update && sudo apt-get install kibana
sudo nano /etc/kibana/kibana.yml
```

Change `server.host: 0.0.0.0`:

```
/etc/kibana/kibana.yml

# Kibana is served by a back end server. This setting specifies the port to use.
#server.port: 5601

# Specifies the address to which the Kibana server will bind. IP addresses and host names are >
# The default is 'localhost', which usually means remote machines will not be able to connect.
# To allow connections from remote users, set this parameter to a non-loopback address.
server.host: 0.0.0.0

# Enables you to specify a path to mount Kibana at if you are running behind a proxy.
# Use the `server.rewriteBasePath` setting to tell Kibana if it should remove the basePath
# from requests it receives, and to prevent a deprecation warning at startup.
# This setting cannot end in a slash.
#server.basePath: ""
```

```bash
sudo service kibana start
sudo service kibana status
```

### Configuring Filebeat

Go to `<ip_address>:5601` on a web browser, go to "Add data", select which type of data to ingest, follow steps on site.

