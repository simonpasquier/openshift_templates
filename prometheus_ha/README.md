These templates allow to deploy a pair of Prometheus/AlertManager instances
for high availability.

The Prometheus servers retrievs the list of AlertManager instances through the
Kubernetes service discovery.

# Compatibility

This example has only be tested with OpenShift 3.7. Note that because of an
[issue](https://github.com/openshift/origin/issues/13401) with DNS resolution,
AlertManager instances won't setup the mesh automatically. To workaround the
problem, you need to add the `hostname` field to the endpoints defintion:

```
$ oc edit endpoints/alertmanager
```

# Setting up the Prometheus account

Create the Prometheus service account with permissions to view services,
endpoints and pods:

```
oc process -f prometheus-service-account.yaml --param NAMESPACE=myproject | oc create -f -
```

# Deploying Prometheus

`prometheus.yaml` deploys a single instance of Prometheus.

```
oc new-app -f prometheus.yaml --param NAMESPACE=myproject
```

Retrieve the hostnames to access the Prometheus and AlertManager UI:

```
$ oc get routes
NAME           HOST/PORT                                 PATH      SERVICES       PORT      TERMINATION   WILDCARD
alertmanager   alertmanager-myproject.10.0.2.15.nip.io             alertmanager   <all>                   None
prometheus     prometheus-myproject.10.0.2.15.nip.io               prometheus     <all>                   None
```

# Deploying the sample application

See [here](../prometheus/README.md#deploying-the-sample-application).
