# notes from [helm k8s live course](https://learning.oreilly.com/live-events/-/0636920074683/0636920096098/)

**helm** is a collection of charts 

Multi-tenancy is multi applications on a one cluster, single-tenancy - one application on one cluster

## basic steps to install/uninstall with helm
follow these [instructions](https://github.com/AdminTurnedDevOps/PearsonCourses/blob/main/Helm-Charts-For-Kubernetes/Segment3/lab.md) to play around with helm


create helm chart
```
helm create nginxtestupdate
```

test the Helm Chart with the dry run command.
```
helm install nginx nginxtestupdate --dry-run --debug
```



list installed instances
```
helm ls -a
```

install/deploy instance 
```
helm install <instance_name> <path_to_chart_folder>/
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

helm history command.
```
helm history <instance_name>
```
