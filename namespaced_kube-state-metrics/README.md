This template deploys the
[kube-state-metrics](https://github.com/kubernetes/kube-state-metrics) to
gather metrics about the resources in the given namespace. Unlike the
[other](../kube-state-metrics) kube-state-metrics template, it doesn't require
any cluster-wide permissions because it doesn't watch all namespaces and system
resources (eg nodes).

# Compatibility

This example has only be tested with OpenShift 3.7.

# Deploying

```
oc new-app -f kube-state-metrics.yaml --param NAMESPACE=myproject
```

Prometheus deployed with the [prometheus_ha](../prometheus_ha) template will
automatically collect the metrics.
