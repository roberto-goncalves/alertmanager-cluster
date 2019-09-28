# Alertmanager-cluster
A simple docker-compose alertmanager cluster with two peers.

What is Alertmanager?

The Alertmanager handles alerts sent by client applications such as the Prometheus server. It takes care of deduplicating, grouping, and routing them to the correct receiver integration such as email, PagerDuty, or OpsGenie. It also takes care of silencing and inhibition of alerts.

From: https://prometheus.io/docs/alerting/alertmanager/

Images used:

* Prometheus: https://github.com/prometheus/prometheus - Dockerhub: https://hub.docker.com/r/prom/prometheus/
* Alertmanager: https://github.com/prometheus/alertmanager - Dockerhub:
https://hub.docker.com/r/prom/alertmanager
* Node-exporter: https://github.com/prometheus/node_exporter - Dockerhub:
https://hub.docker.com/r/prom/node-exporter
* Python-flask-api: https://github.com/roberto-goncalves/python-flask-api - Dockerhub:
https://hub.docker.com/r/nonick70/python-flask-api

How to use:

```
docker-compose up
```

Results:

If everything is correct, after 30 seconds on InstanceUp(node_exporter) the cluster will automatically deduplicate the message from Prometheus and send via Webhook to the application, which will fetch the message and return with:
```
application_1    | 172.26.0.5 - - [28/Sep/2019 19:37:37] "POST / HTTP/1.1" 200 -
```
Then, to test on alertmanager's standalone mode just comment out --cluster.peers from both on docker-compose.yml, the results shoud be:
```
application_1    | 172.26.0.5 - - [28/Sep/2019 19:37:37] "POST / HTTP/1.1" 200 -
application_1    | 172.26.0.2 - - [28/Sep/2019 19:37:37] "POST / HTTP/1.1" 200 
```
So, the deduplication is working and we have HA mode.
