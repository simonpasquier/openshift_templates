# Prometheus

`prometheus.yaml` deploys a single instance of Prometheus
that can monitor applications running in a given project without requiring
cluster-wide permissions for the Prometheus service account.

It requires admin permissions on the namespace to be deployed successfully.

```
oc new-app -f prometheus.yaml --param NAMESPACE=myproject
```

Retrieve the hostname to access the Prometheus UI:

```
$ oc get route
NAME         HOST/PORT                               PATH      SERVICES     PORT      TERMINATION   WILDCARD
prometheus   prometheus-myproject.127.0.0.1.nip.io             prometheus   <all>                   None
```

# Sample application

`sample_app.yaml` deploys a sample application that will be automatically
monitored by Prometheus. It can be deployed by any user of the namespace.

```
oc new-app -f prometheus.yaml --param NAMESPACE=myproject
```

After a while, you should see that Prometheus has discovered 2 new targets.
