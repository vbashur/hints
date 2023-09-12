# install k8s

install minicube https://minikube.sigs.k8s.io/docs/start/
```
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-darwin-amd64
sudo install minikube-darwin-amd64 /usr/local/bin/minikube
minikube start
minikube status
kubectl get all -A
```

# terms

> k8s is a cloud native computig platform that solves the challenge tol have the application succesfully deployed and manaeged on a cloud infrastructure. It treats the applications as containers.

**configmap** - how to deal with the configuration

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




