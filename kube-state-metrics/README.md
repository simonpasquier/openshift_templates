This template deploys the
[kube-state-metrics](https://github.com/kubernetes/kube-state-metrics) service
behind an [OAuth proxy](https://github.com/openshift/oauth-proxy).

# Compatibility

This example has only be tested with OpenShift 3.7.

# Deploying


```
oc new-app -f kube-state-metrics.yaml
```
