These templates allow to deploy a pair of Prometheus/AlertManager instances
for high availability. OAuth proxies run in front of the services for
authentication/authorization.

Note that the Prometheus servers retrieve the list of AlertManager instances
through the Kubernetes service discovery.

# Compatibility

This example has only be tested with OpenShift 3.7.

# Setting up the Prometheus account

Create the Prometheus service account:

```
oc process -f prometheus-service-account.yaml --param NAMESPACE=myproject | oc create -f -
```

The service account is granted the system-auth-delegator role because this is
what OAuth Proxy needs to validate bearer tokens.

# Deploying Prometheus

`prometheus.yaml` deploys 2 Prometheus and 2 AlertManager as StatefulSets.

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
