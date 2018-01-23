These templates allow to deploy Prometheus with AlertManager and get alerts
stored into Elasticsearch. Kibana will be used to visualize and query the
alerts.

# Compatibility

This example has only be tested with OpenShift 3.7.

# Setting up the Prometheus account

Create the Prometheus service account with permissions to view services,
endpoints and pods:

```
oc process -f prometheus-service-account.yaml --param NAMESPACE=myproject | oc create -f -
```

# Deploying Prometheus and others

For Elasticsearch to start, you need to increase some kernel parameters:

```
sudo sysctl -w vm.max_map_count=262144
```

`prometheus.yaml` deploys a single instance of Prometheus, Elasticsearch,
AlertManager and
[alertmanager2es](https://github.com/cloudflare/alertmanager2es).


```
oc new-app -f prometheus.yaml --param NAMESPACE=myproject
```

Retrieve the routes to access the Prometheus UI:

```
$ oc get route
NAME            HOST/PORT                                     PATH      SERVICES        PORT      TERMINATION   WILDCARD
alertmanager    alertmanager-myproject.192.168.1.17.nip.io              alertmanager    <all>                   None
elasticsearch   elasticsearch-myproject.192.168.1.17.nip.io             elasticsearch   <all>                   None
prometheus      prometheus-myproject.192.168.1.17.nip.io                prometheus      <all>                   None
```

# Deploying Kibana

Because of file permission issues, it isn't possible to deploy the Elastic
Kibana image on OpenShift. Instead start it directly using the Docker CLI.

```
docker run --name kibana -p 5601:5601 -e "ELASTICSEARCH_URL=http://$(oc get svc/elasticsearch -o="jsonpath={.spec.clusterIP}"):9200" --rm docker.elastic.co/kibana/kibana-oss:6.1.2
```

The Kibana UI is at <http://localhost:5601/>. Go to the Management tab and
click on "Index Patterns" to configure the alerts index.

# Deploying the sample application

See [here](../prometheus/README.md#deploying-the-sample-application).
