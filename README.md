# Docker ELK stack

Run the latest version of the ELK (Elasticsearch, Logstash, Kibana) stack with Docker and Docker Compose.

It will give you the ability to analyze any data set by using the searching/aggregation capabilities of Elasticsearch
and the visualization power of Kibana.

Based on the official Docker images:

* [elasticsearch](https://github.com/elastic/elasticsearch-docker)
* [logstash](https://github.com/elastic/logstash-docker)
* [kibana](https://github.com/elastic/kibana-docker)

## Requirements

### Host setup

1. Install Docker
2. Install Docker Compose
3. Clone this repository

## Usage

### Bringing up the stack

Start the ELK stack using `docker-compose`:

```bash
$ docker-compose up
```

You can also choose to run it in background (detached mode):

```bash
$ docker-compose up -d
```

Give Kibana about 2 minutes to initialize, then access the Kibana web UI by hitting
http://<IP>:5601 with a web browser.

By default, the stack exposes the following ports:
* 5000: Logstash TCP input.
* 9200: Elasticsearch HTTP
* 9300: Elasticsearch TCP transport
* 5601: Kibana

Now that the stack is running, you will want to inject some log entries. The shipped Logstash configuration allows you
to send content via TCP:

```bash
$ nc localhost 5000 < /path/to/logfile.log
```

## Initial setup

### Default Kibana index pattern creation

When Kibana launches for the first time, it is not configured with any index pattern.

#### Via the Kibana web UI

#### On the command line
Run this command to create a Logstash index pattern:

```bash
$ curl -XPUT -D- 'http://localhost:9200/.kibana/index-pattern/logstash-*' \
    -H 'Content-Type: application/json' \
    -d '{"title" : "logstash-*", "timeFieldName": "@timestamp", "notExpandable": true}'
```

This command will mark the Logstash index pattern as the default index pattern:

```bash
$ curl -XPUT -D- 'http://localhost:9200/.kibana/config/5.6.2' \
    -H 'Content-Type: application/json' \
    -d '{"defaultIndex": "logstash-*"}'
```

## Configuration

**NOTE**: Configuration is not dynamically reloaded, you will need to restart the stack after any change in the
configuration of a component.


