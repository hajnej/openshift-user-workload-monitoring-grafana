# Grafana example for OpenShift User Workload Monitoring

This repo contains a Kustomize base and example overlay which deploys a
proof-of-concept Grafana instance which is configured to "plug-in" to OpenShift
Container Platform's [user workload monitoring stack][user-workload-monitoring].

The generated manifests have been tested against OpenShift Container Platform
v4.5.12. Breaking changes in higher versions may also break this Grafana
instance.

## Why?

Because the user workload monitoring stack is still an early tech-preview, there
is no support for user-provided Grafana dashboards. Even though metrics scraping
and alerting is provided out-of-the-box, most of the applications/operators I
support need a friendly dashboard to take up screen real-estate :grin:.

## How?

These manifests assume that Grafana should be installed into a different
namespace from your target application. In other words, given the example
application namespace `ns1` (as used in the example `overlay`), Grafana will be
deployed to the `app-monitoring` namespace and will be given permission to query
metrics scraped from the `ns1` namespace.

Puzzle pieces which comprise this example:
- [OpenShift OAuth proxy][oauth-proxy]
- [OpenShift Service Serving Signer][service-signer]

It's also important to understand how RBAC is used to enforce tenancy in
OpenShift's user workload monitoring stack (specifically how `kube-rbac-proxy`
and `prom-label-proxy` enforce namespace permissions):
- [OpenShift user workload monitoring enhancement proposal][uwm-proposal]

## Install

Edit all of the `kustomization.yaml` files in the `overlay` directory and adapt
the namespaces to match your application and monitoring namespace.

This POC doesn't include any Grafana dashboards, so you'll have to patch your
own in to a configmap and the Grafana deployment.


[user-workload-monitoring]:https://docs.openshift.com/container-platform/4.5/monitoring/monitoring-your-own-services.html
[oauth-proxy]:https://github.com/openshift/oauth-proxy
[service-signer]:https://docs.openshift.com/container-platform/4.5/security/certificates/service-serving-certificate.html
[uwm-proposal]:https://github.com/openshift/enhancements/blob/master/enhancements/monitoring/user-workload-monitoring.md