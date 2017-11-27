These templates allow to deploy a single Prometheus instance that can monitor
applications in a given namespace.

Depending on your grants, it can either:

* Monitor only the application metrics (it requires only the ability to create
  roles and bindings in the namespace).
* Monitor the application and container metrics (it requires the ability to
  create cluster-wide roles and bindings).

# Compatibility

This example has only be tested with OpenShift 3.6. In particular, it will
require some changes for Kubernetes >= 1.7.3 since the 'container_' metrics are
now exposed by the Kubelet service at /metrics/cadvisor instead of /metrics.

# Setting up the Prometheus account

Create the Prometheus service account with permissions to view services,
endpoints and pods:

```
oc process -f prometheus-service-account.yaml --param NAMESPACE=myproject | oc create -f -
```

Add permissions to view the nodes resources if you want to get the container
metrics:

```
oc process -f node-monitoring-permissions.yaml --param NAMESPACE=myproject | oc create -f -
```

# Deploying Prometheus

`prometheus.yaml` deploys a single instance of Prometheus.

With application metrics only:

```
oc new-app -f prometheus.yaml --param NAMESPACE=myproject
```

With container and application metrics only:

```
oc new-app -f prometheus.yaml --param NAMESPACE=myproject --param PROMETHEUS_CONFIG=full-monitoring
```

Retrieve the hostname to access the Prometheus UI:

```
$ oc get route
NAME         HOST/PORT                               PATH      SERVICES     PORT      TERMINATION   WILDCARD
prometheus   prometheus-myproject.127.0.0.1.nip.io             prometheus   <all>                   None
```

# Deploying the sample application

`sample_app.yaml` deploys a sample application with 2 replicas that will be
automatically monitored by Prometheus. It can be deployed by any user of the
namespace.

```
oc new-app -f sample-app.yaml
```

After a while, you should see that Prometheus has discovered 2 new targets.

You can also scale up and down the number of pods up and check that Prometheus
adds/removes targets consequently.
