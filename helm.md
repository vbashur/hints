# notes from [helm k8s live course](https://learning.oreilly.com/live-events/-/0636920074683/0636920096098/)

**helm** is a collection of charts 

Multi-tenancy is multi application on a one cluster, single-tenancy - one application on one cluster




list installed instances

```
helm ls -a
```

uninstall instance
```
helm uninstall <instance_name>
```

build helm dependencies
```
helm dependency build <path_to_values_file>
```

helm installation
```
helm upgrade --wait --install --atomic --namespace <target_namespace> --set best-secret-base.fullnameOverride=<app_name> -f ./values.yaml --timeout 10m0s <app_name> .
```
