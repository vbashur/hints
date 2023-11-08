# install k8s

[install minicube](https://minikube.sigs.k8s.io/docs/start/)
```
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-darwin-amd64
sudo install minikube-darwin-amd64 /usr/local/bin/minikube
minikube start
minikube status
kubectl get all -A
```

# terms

> k8s is a cloud native computig platform that solves the challenge to have the application succesfully deployed and managed on a cloud infrastructure. It treats the applications as containers, particulary pods.

> pods - the basic unit in Kubernetes, represents one or more containers that share common resources

> deployments- the application itself, standard entity that is rolled out with Kubernetes

Do **NOT** run standalone Pods, run deployments only

**configmap** - how to deal with storing configuration and other specific parameters in a cloud environment

**ingress** - how to access

**persistence volumes** - where to store the data

**container runtime** - programm that runs the container, it grabs an image from image registry

**on premise** (на территории) - On-premise software is installed on local servers or computers, while off-premise software is hosted on remote servers or in the cloud

**namespace** - isolated environment to run the application

CKAD - certification for developers (begin), minikube suits for self-preparations

# basic commands

## minikube

`minikube status` - check the minikube status

`minikube ssh` - go with ssh into cluster network

`minikube ip` - get the ip address of minikube

`minikube dashboard` - launch minikube dashboard

`minikube addons list` - list minikube addons

`minikube addons enable ingress` - enable minikube addons (e.g. ingresss)

## kubectl

`kubectl get all -A` - get pods running

`kubectl run appone --image=nginx` - launch the simple app

`kubectl create` - create a resource from file or stdin 

`kubectl create deployment -h | less` - shows possible completions (e.g. `kubectl create deployment apptwo --image=nginx --replicas=3`)

`kubectl delete pod <pod_name>' - delete the pod manually

`kubectl describe pod <pod_name> | less` - show the pod information

`kubectl explain pod.spec | less` - print some comprehensive help about pod configuration

`kubectl api-resources` -  print some comprehensive help about _resources_ configuration

`kubectl get all --selector app=apptwo -o wide` - get the information about the particular pod 

`kubectl get deploy apptwo -o yaml | less` - get the info about the pod in yml format

`kubectl apply -f myngnix.yaml` - create a pod from yml file, or applies the settings to existed pod

`kubectl describe pods mymariadb` - get the pod status

`kubectl logs mymariadb-5448d5bdfc-cjgv8` - read the logs of the particular container

`kubectl set env deploy/mymariadb MARIADB_ROOT_PASSWORD=password` - set the env variable to the pod

`kubectl scale deploy nginxsvc --replicas=3` - scale the pod 

`kubectl expose deploy nginxsvc --port=80` - expose the port for the deploy

`kubectl delete all --all` - remove all k8s resources in default namespace

# k8s resources
**pod** - minimal entity managed by k8s, pod starts from containers and can have some volumes
**deployment** adds scalability and updates to pods, within `--image=nginx --replicas=3` args we added three pods (that are named 'replicaset' in total)

# k8s networking
sevice resource - represents on a cluster network which pod is accessible. Serive provides a 'cluster IP' and 'Node port' 
node port - exposes port 
ingress - k8s integrated http/https proxy

`kubectl describe svc nginxsvc` - show the service information ('nginxsvc' is a deploy name here)

`kubectl get endpoints` - show all endpoints

 `kubectl edit svc nginxsvc` - edit the service information ('nginxsvc' is a deploy name here)

## ingress
`kubectl create ingress simple --rule="foo.com/bar=svc1:8080,tls=my-cert"` - create ingress with a rule

`kubectl  create ingress nginxsvc --rule "foo.com/=nginxsvc:80"` - resolve foo.com to nginxsvc:80

`kubectl describe ing nginxsvc` - check the status of ingress for a service

## volumes
pvc - persistent volume
storage provisioner - application that works with 'storage class' resources, storage class allocates persistent volume

config map - special resource that you store in k8s that is used by your environment where you want to have your application



# Cheatsheet :robot:

### context and configuration
| command                                                           | description                | example                                        |
|-------------------------------------------------------------------|----------------------------|------------------------------------------------|
| kubectl config get-contexts                                       | 	Display list of contexts	 |                                                |
| kubectl config current-context                                    | Display the current-context|                                                |
| kubectl config use-context <cluster-name>	                        | Change the current cluster| kubectl config use-context bs252-d-euw-aks-hey | 
| kubectl config set-context --current --namespace=<namespace_name> | Set the default namespace to <namespace_name> | |  
| kubectl config view                                               | Display configuration| |
| kubectl get pods -n <namespace_name>                              | List all pods in a namespace | |
| kubectl describe pods <pod_name>                                  | Describe commands with verbose output | |

### logs 

| command                                                           | description                | example                                        |
|-------------------------------------------------------------------|----------------------------|------------------------------------------------|
| kubectl -n <namespace> get events --sort-by='{.lastTimestamp}' | Print latest events in a namespace | |
| kubectl logs <pod_name> |	Print the logs for a pod | |
| kubectl logs --since=6h <pod_name> | Print the logs for the last 6 hours for a pod | |
| kubectl logs --tail=50 <pod_name> | 	Get the most recent 50 lines of logs for a pod | |
| kubectl logs -f <pod_name> | Print the logs for a pod and follow new logs | |
| kubectl logs <pod_name> --all-containers | View the logs for all containers in a pod | |

### executing in running pods
| command                                                           | description                | 
|-------------------------------------------------------------------|----------------------------|
| kubectl exec -it <pod_name> -- /bin/bash | Open a bash command shell |
| kubectl exec -it <pod_name> -- /bin/bash -c "command(s)" | kubectl exec -it <pod_name> -- /bin/bash -c "command(s)" |
| kubectl rollout restart <pod_name> | restart a pod |











