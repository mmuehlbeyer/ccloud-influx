# ccloud-influx

Demo Setup for Scraping Confluent Cloud Metrics (Metrics API) with Telegraf and save them to influxdb.

### 1. Prerequisites

* docker-compose
* access to Confluent Cloud
* a Confluent Cloud Cluster 

### 2. create Cloud API Key and Secret

create with Confluent CLI or Cloud UI

```bash
confluent api-key create --resource cloud
```

Save both the api key and the secret

Output will look like
```
+------------+------------------------------------------------------------------+
| API Key    | 3RDR6YRBVOG3S4MP                                                 |
| API Secret | lkmqOGxtdQ+PuVq8gD+hmTWpeettHZKAZV3y72Ic7cMYirKf2sj86cFk86O52ZfB |
+------------+------------------------------------------------------------------+
```

### 3. set the required env variables

```bash
export CCLOUD_METRICS_API_KEY="3RDR6YRBVOG3S4MP"
export CCLOUD_METRICS_API_SECRET="lkmqOGxtdQ+PuVq8gD+hmTWpeettHZKAZV3y72Ic7cMYirKf2sj86cFk86O52ZfB"
export DOCKER_INFLUXDB_INIT_ORG=demo-org
export DOCKER_INFLUXDB_INIT_BUCKET=demo-bucket
export DOCKER_INFLUXDB_INIT_ADMIN_TOKEN=testtoken
```

### 4. adapt telegraf.conf 

adapt line 148
set to your cluster id, s.th like lkc-xxxxxx

```
urls = ["https://api.telemetry.confluent.cloud/v2/metrics/cloud/export?resource.kafka.id=lkc-z310w0"]
```

### 5. start the stack with

```bash
docker-compose up -d 
```

check the logs for the both container started.

### 6. check the collected metrics via influxdb ui

point you browser to


[http://localhost:8086](http://localhost:8086)


login with the credentials set in docker-compose.yml (user:root, pw:welcome12)

check the bucket for incoming data and query it

![influx01](/assets/influx01.png)

Happy testing!


### Refs

https://docs.confluent.io/confluent-cli/current/command-reference/api-key/confluent_api-key_create.html

https://www.influxdata.com/blog/running-influxdb-2-0-and-telegraf-using-docker/