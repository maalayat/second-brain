## How to install kubernetes
snap install kubectl --classic
kubectl version --client
kubectl help

## How to install minikube
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube
minikube version

## Getting start with minikube and kubernets
minikube start
minikube status
kubectl version
kubectl get nodes
kubectl cluster-info

## Run
kubectl run hello-world --image=fiunchinho/codely-docker:latest --port=80
kubectl port-forward deployment/hello-world 8000:80 # bad, not working
kubectl port-forward hello-world 8000:80

### Print output yaml object
kubectl run hello-world --image=fiunchinho/codely-docker:latest  --restart=Never --port=80 --dry-run -o yaml
outpunt: --dry-run is deprecated and can be replaced with --dry-run=client

### Create a pod.yaml from kubectl generator
#### Or you can print yaml generated
kubectl run hello-world --image=fiunchinho/codely-docker:latest  --restart=Never --port=80 --dry-run -o yaml
#### Or you can save yaml generated in a file
kubectl run hello-world --image=fiunchinho/codely-docker:latest  --restart=Never --port=80 --dry-run -o yaml > pod.yaml

### Create a service from kubectl generator
kubectl expose pod/<name_pod> --port 80
#### Or you can print yaml generated
kubectl expose pod/hello-world --port 80 --dry-run -o yaml

### Send to kubernetes API
kubectl create -f pod.yaml

### List all pods
kubectl get pods

### List all services
kubectl get svc

### Describe status pod
kubectl describe pod hello-world

### Delete a pod
kubectl delete pod <pod_name>

### Delete a service
```
kubectl delete svc <pod_name>
```

### Edit a pod
```
kubectl edit pod <pod_name>
```

## Setup minikube
```
minikube config set memory 4096
```

### Getting minikube IP
```
minikube ip
```

## Deployments
### Create deployment file
```
kubectl create deployment hello-world --image=fiunchinho/codely-docker:latest --port=80 --dry-run=clie  
nt -o yaml > deployment.yaml
```
### Sending to Kubernetes API
```
kubectl create -f deployment.yaml
```
### Getting deployments
```
kubectl get deployments  
NAME          READY   UP-TO-DATE   AVAILABLE   AGE  
hello-world   1/1     1            1           56s
```
### Editing a deployment
```
kubectl edit deployment hello-world
```
```
kubectl scale deployments hello-world --replicas=3
```
### Listing deployments and services
```
kubectl get deploy,pod
```
### Expose a deployment
```
kubectl expose deployment/hello-world --port 80 --type=NodePort
```
## Ingress

### List all minikube addons
```
minikube addons list
```
### Enabling Ingress addon
```
minikube addons enable ingress
```
### Creating ingress file
```
kubectl create ingress hello-world --rule="codely-k8s.com/*=hello-world:80" --dry-run -o yaml > ingress.yaml
```
### Sending to Kubernetes API
```
kubectl create -f ingress.yaml
```
### Getting ingress
```
kubectl get ingress
```
### Describing ingress
```
kubectl describe ingress hello-world
```

## Config maps
### Creating configmap

```
kubectl create configmap special-config --from-literal=example.property1=hello --from-literal=example.property2=world
```
### Getting configmap
```
kubectl get configmaps
```
### Getting configmap in yaml format
```
kubectl get configmaps special-config -o yaml
```
### Editing configmap
```
kubectl edit configmap special-config
```
### Sending to Kubernetes API
```
kubectl create -f pod_config_env.yaml
```
### Exec commands in a pod
```
kubectl exec -it hello-world -- /bin/bash
```
### Creating configmap from file
```
kubectl create configmap nginx-conf --from-file nginx.conf
```

## Secrets
### Creating secret
```
kubectl create secret generic test-secret --from-literal=username='my-app' --from-literal=password='323434$jas'
```
### Getting secrets
```
kubectl get secrets
```
### Getting secret in yaml format
```
kubectl get secret test-secret -o yaml
```
### Decode user
```
echo "bXktYXBw" | base64 -d
```
### Sending to Kubernetes API
```
kubectl create -f pod_secret_env.yaml
```
### Recreating pod with volumes
```
kubectl create -f pod_secret_volume.yaml
```
### Exec commands in Pod
```
kubectl exec -it hello-world -- ls /etc/secret-volume
```
```
kubectl exec -it hello-world -- cat /etc/secret-volume/username
```
## Kubctl config
```
kubectl config view
```
### Kubectl file path
```
$HOME/.kube/config
```
### Changing context
```
kubectl config set-context minikube
```

## Complete example

## Generate  pod yaml
```
kubectl run hello-world --image=fiunchinho/codely-docker:latest --restart=Never --port=80 --dry-run -o yaml > pod.yaml
```

### Editing, adding labels (codely:pro)
```
apiVersion: v1  
kind: Pod  
metadata:  
 creationTimestamp: null  
 labels:  
   run: hello-world  
   codely: pro  
 name: hello-world  
spec:  
 containers:  
 - image: fiunchinho/codely-docker:latest  
   name: hello-world
```

### Create a pod
```
kubectl create -f pod.yaml
```
### List all pods
```
kubectl get pods  
NAME          READY   STATUS    RESTARTS   AGE  
hello-world   1/1     Running   0          30s
```

## Generate  and create service

### ClusterIp

#### Expose ClusterIP service
```
kubectl expose pod/hello-world --port 80
```

### Get services (in other console)
```
$ kubectl get svc
NAME          TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)   AGE  
hello-world   ClusterIP   10.109.219.39   <none>        80/TCP    13m  
kubernetes    ClusterIP   10.96.0.1       <none>        443/TCP   5d22h
```

#### Run my-client-app from alpine image with a shell
```
kubectl run --rm -i --tty my-client-app --image=alpine --restart=Never -- sh
```

### Request from alpine console
```
wget hello-world/index.php -qSO-  
 HTTP/1.1 200 OK  
 Date: Mon, 19 Dec 2022 21:46:54 GMT  
 Server: Apache/2.4.25 (Debian)  
 X-Powered-By: PHP/7.2.3  
 Content-Length: 15  
 Connection: close  
 Content-Type: text/html; charset=UTF-8  
    
Hello CodelyTV!
```
#### Where:
wget:
-q: remove headers
SO-: standar output

#### Request from alpine console using IP
```
wget 10.109.219.39/index.php -qSO-
```

### NodePort

#### Expose NodePort service
```
kubectl expose pod/hello-world --port 80 --type=NodePort
```
#### Getting minikube IP
```
$ minikube ip
$ 192.168.49.2
```
#### Calling service
```
curl -i 192.168.49.2
```
#### Open in a browser
```
minikube service hello-world
```

### Link local docker to minikube docker
eval $(minikube docker-env)
eval $(minikube docker-env -u)

## Deploying a clouster 

### Azure

1. Config kubectl in your local pc https://learn.microsoft.com/es-mx/azure/aks/learn/quick-kubernetes-deploy-portal?tabs=azure-cli
2. Changing to azure context
3. `kubectl run hello-world --image=fiunchinho/codely-docker:latest --port=80`
4. `kubectl get deploy,pod`
5. `kubectl expose deployment/hello-world --port 80 --type=LoadBalancer`
6. `kubectl get svc`
7. `kubectl describe svc hello-world`
8. `kubectl scale deployments hello-world --replicas=3`
9. `kubectl describe pod hello-world-7c7875b554-b`

## GCP
1. Install gcloud https://cloud.google.com/cli
2. Setup a project https://cloud.google.com/kubernetes-engine/docs/deploy-app-cluster
3. Changing to GCP context: `gcloud container clusters get-credentials codely --zone europe-west1-b --project codely-218919`
4. `kubectl get nodes`
5. `kubectl run hello-world --image=fiunchinho/codely-docker:latest --port=80`
6. `kubectl get pods -w`
7. `kubectl expose deployment/hello-world --port 80 --type=LoadBalancer`
8. `kubectl get svc,deploy,pod`
9. `kubectl describe service hello-world`
10. `kubectl describe pod hello-world-7c7875b554-b`

## Troubleshooting

E1220 18:20:38.725644 37419 start.go:210] Error parsing version semver: Version string empty
[https://github.com/kubernetes/minikube/issues/4088](https://github.com/kubernetes/minikube/issues/4088)
[https://github.com/kubernetes/minikube/issues/2753](https://github.com/kubernetes/minikube/issues/2753)

## Timeout NodePort
```
curl -i 192.168.49.2:32592  
curl: (28) Failed to connect to 192.168.49.2 port 32592 after 129929 ms: Connection timed out
```
### Solution
#### Delete minikube
```
minikube delete
```
#### Create a new minikube with virtualbox driver
```
minikube start --driver=virtualbox
```
#### Getting a new minikube IP
```
minikube ip
```

## Other local Kubernetes
- [Kind](https://kind.sigs.k8s.io/)
- [MicroK8s](https://microk8s.io/)
- [Kubernetes on docker-desktop](https://docs.docker.com/desktop/kubernetes/)

## Links
- https://docs.microsoft.com/es-mx/azure/aks/kubernetes-walkthrough-portal
- https://learn.microsoft.com/es-mx/azure/aks/learn/quick-kubernetes-deploy-portal?tabs=azure-cli
- https://kubedex.com/google-gke-vs-microsoft-aks-vs-amazon-eks/