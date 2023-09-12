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

CKAD - certification for developers (begin), minikube suits for self-preparations

# basic commands
`kubectl get all -A` - get pods running

`minikube status` - check the minikube status

`minikube dashboard` - launch minikube dashboard

`kubectl run appone --image=nginx` - launch the simple app







