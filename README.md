# helm-ingress

Release provides a Helm chart for generating an ingress for your application. This chart allows you to expose services in your helm charts for which we do not automatically create an ingress or hostname. Find our more in our [helm documentation](https://docs.releasehub.com/reference-guide/helm).

You can either copy the contents of the helm-repo into your source control repository or reference the Helm chart using a remote repo chart

```yaml
charts:
- name: <service>-ingress
  add: release-ingress
  repo_url: https://raw.githubusercontent.com/releasehub-com/helm-ingress/main/
  directory: <path in your repo to values.yaml>
  install: release-ingress/release-ingress
  values: values.yaml
```

# IMPORTANT VERSION INFORMATION

Kubernetes has deprecated the API for `ingress` type as of v1.19 and removed the [deprecated version](https://kubernetes.io/blog/2021/07/14/upcoming-changes-in-kubernetes-1-22/#what-to-do) as of v1.22.
We have introduced this breaking change internally for all customers who upgrade to v1.21. This means that you should use the v1.20 tag for cluster versions up to v1.20 and use the main branch or later releases
for versions v1.21 and above. The reason for this change is that customers who use v1.20 of the chart will be able to upgrade to v1.21 of Kubernetes without breaking any existing ingress charts. Once upgraded,
the v1.21 chart will need to be used for any new ingress templates. This gives customers a smooth upgrade path without breaking exisitng infrastructure and applications. When v1.22 and above are available,
customers will be able to use only the new chart version.

The example for using v1.20 is similar to the above:
```yaml
charts:
  version: 2.1.0
  ...
```

# Values File Example
To use the Helm chart you will need to have a `values.yaml` file located in your source control repository. You will need to customize the `values.yaml` file to reference the service that you would like to expose to the Internet.

```yaml
service:
  name: frontend
  externalPort: 5000
ingress:
  hosts:
    - ${FRONTEND_INGRESS_HOST}
  annotations:
    kubernetes.io/ingress.class: nginx
    nginx.ingress.kubernetes.io/force-ssl-redirect: "true"
```
