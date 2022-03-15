# helm-ingress

Release provides a Helm chart for generating an ingress for your application. This chart allows you to expose services in your helm charts for which we do not automatically create an ingress or hostname. Find our more in our [helm documentation](https://docs.releasehub.com/reference-guide/helm).

You can either copy the contents of the helm-repo into your source control repository or reference the Helm chart using a remote repo chart

```yaml
charts:
- name: ingress
  repo_url: https://github.com/releasehub-com/helm-ingress.git
  directory: helm/release-ingress
  values: values.yaml
```

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
