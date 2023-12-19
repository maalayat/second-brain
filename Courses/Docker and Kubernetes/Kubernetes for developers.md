## How to install kubernetes
```bash
snap install kubectl --classic
kubectl version --client
kubectl help
```
## How to install minikube
```bash
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube
minikube version
```
## Getting start with minikube and kubernets
```bash
minikube start
minikube status
kubectl version
kubectl get nodes
kubectl cluster-info
```
## Run
```bash
kubectl run hello-world --image=fiunchinho/codely-docker:latest --port=80
kubectl port-forward deployment/hello-world 8000:80 # bad, not working
kubectl port-forward hello-world 8000:80
```
### Print output yaml object
```bash
kubectl run hello-world --image=fiunchinho/codely-docker:latest  --restart=Never --port=80 --dry-run -o yaml
outpunt: --dry-run is deprecated and can be replaced with --dry-run=client
```
### Create a pod.yaml from kubectl generator
#### Or you can print yaml generated
```bash
kubectl run hello-world --image=fiunchinho/codely-docker:latest  --restart=Never --port=80 --dry-run -o yaml
```
#### Or you can save yaml generated in a file
```bash
kubectl run hello-world --image=fiunchinho/codely-docker:latest  --restart=Never --port=80 --dry-run -o yaml > pod.yaml
```
### Create a service from kubectl generator
```bash
kubectl expose pod/<name_pod> --port 80
```
#### Or you can print yaml generated
```bash
kubectl expose pod/hello-world --port 80 --dry-run -o yaml
```
### Send to kubernetes API
```bash
kubectl create -f pod.yaml
```
### List all pods
```bash
kubectl get pods
```
### List all services
```bash
kubectl get svc
```
### Describe status pod
```bash
kubectl describe pod hello-world
```
### Delete a pod
```bash
kubectl delete pod <pod_name>
```
### Delete a service
```bash
kubectl delete svc <pod_name>
```

### Edit a pod
```bash
kubectl edit pod <pod_name>
```
## Setup minikube
```bash
minikube config set memory 4096
```
### Getting minikube IP
```bash
minikube ip
```

## Deployments
### Create deployment file
```bash
kubectl create deployment hello-world --image=fiunchinho/codely-docker:latest --port=80 --dry-run=clie  
nt -o yaml > deployment.yaml
```
### Sending to Kubernetes API
```bash
kubectl create -f deployment.yaml
```
### Getting deployments
```bash
kubectl get deployments  
NAME          READY   UP-TO-DATE   AVAILABLE   AGE  
hello-world   1/1     1            1           56s
```
### Editing a deployment
```bash
kubectl edit deployment hello-world
```
```
kubectl scale deployments hello-world --replicas=3
```
### Listing deployments and services
```bash
kubectl get deploy,pod
```
### Expose a deployment
```bash
kubectl expose deployment/hello-world --port 80 --type=NodePort
```
## Ingress

### List all minikube addons
```bash
minikube addons list
```
### Enabling Ingress addon
```bash
minikube addons enable ingress
```
### Creating ingress file
```bash
kubectl create ingress hello-world --rule="codely-k8s.com/*=hello-world:80" --dry-run -o yaml > ingress.yaml
```
### Sending to Kubernetes API
```bash
kubectl create -f ingress.yaml
```
### Getting ingress
```bash
kubectl get ingress
```
### Describing ingress
```bash
kubectl describe ingress hello-world
```

## Config maps
### Creating configmap

```bash
kubectl create configmap special-config --from-literal=example.property1=hello --from-literal=example.property2=world
```
### Getting configmap
```bash
kubectl get configmaps
```
### Getting configmap in yaml format
```bash
kubectl get configmaps special-config -o yaml
```
### Editing configmap
```bash
kubectl edit configmap special-config
```
### Sending to Kubernetes API
```bash
kubectl create -f pod_config_env.yaml
```
### Exec commands in a pod
```bash
kubectl exec -it hello-world -- /bin/bash
```
### Creating configmap from file
```bash
kubectl create configmap nginx-conf --from-file nginx.conf
```

## Secrets
### Creating secret
```bash
kubectl create secret generic test-secret --from-literal=username='my-app' --from-literal=password='323434$jas'
```
### Getting secrets
```bash
kubectl get secrets
```
### Getting secret in yaml format
```bash
kubectl get secret test-secret -o yaml
```
### Decode user
```bash
echo "bXktYXBw" | base64 -d
```
### Sending to Kubernetes API
```bash
kubectl create -f pod_secret_env.yaml
```
### Recreating pod with volumes
```bash
kubectl create -f pod_secret_volume.yaml
```
### Exec commands in Pod
```bash
kubectl exec -it hello-world -- ls /etc/secret-volume
```
```bash
kubectl exec -it hello-world -- cat /etc/secret-volume/username
```
## Kubctl config
```bash
kubectl config view
```
### Kubectl file path
```bash
$HOME/.kube/config
```
### Changing context
```bash
kubectl config set-context minikube
```

## Complete example

## Generate  pod yaml
```bash
kubectl run hello-world --image=fiunchinho/codely-docker:latest --restart=Never --port=80 --dry-run -o yaml > pod.yaml
```

### Editing, adding labels (codely:pro)
```bash
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
```bash
kubectl create -f pod.yaml
```
### List all pods
```bash
kubectl get pods  
NAME          READY   STATUS    RESTARTS   AGE  
hello-world   1/1     Running   0          30s
```

## Generate  and create service

### ClusterIp

#### Expose ClusterIP service
```bash
kubectl expose pod/hello-world --port 80
```

### Get services (in other console)
```bash
$ kubectl get svc
NAME          TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)   AGE  
hello-world   ClusterIP   10.109.219.39   <none>        80/TCP    13m  
kubernetes    ClusterIP   10.96.0.1       <none>        443/TCP   5d22h
```

#### Run my-client-app from alpine image with a shell
```bash
kubectl run --rm -i --tty my-client-app --image=alpine --restart=Never -- sh
```

### Request from alpine console
```bash
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
```bash
wget 10.109.219.39/index.php -qSO-
```

### NodePort

#### Expose NodePort service
```bash
kubectl expose pod/hello-world --port 80 --type=NodePort
```
#### Getting minikube IP
```bash
$ minikube ip
$ 192.168.49.2
```
#### Calling service
```bash
curl -i 192.168.49.2
```
#### Open in a browser
```bash
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
```bash
curl -i 192.168.49.2:32592  
curl: (28) Failed to connect to 192.168.49.2 port 32592 after 129929 ms: Connection timed out
```
### Solution
#### Delete minikube
```bash
minikube delete
```
#### Create a new minikube with virtualbox driver
```bash
minikube start --driver=virtualbox
```
#### Getting a new minikube IP
```bash
minikube ip
```

## HTTP Trace: Dial to tcp:<> failed: dial tcp <>: connect: no route to host
```bash
I1214 13:09:41.864721   44369 round_trippers.go:508] HTTP Trace: Dial to tcp:192.168.49.2:8443 failed: dial tcp 192.168.49.2:8443: connect: no route to host  
I1214 13:09:41.864843   44369 round_trippers.go:553] GET https://192.168.49.2:8443/api?timeout=32s  in 3072 milliseconds  
I1214 13:09:41.864884   44369 round_trippers.go:570] HTTP Statistics: DNSLookup 0 ms Dial 3071 ms TLSHandshake 0 ms Duration 3072 ms  
I1214 13:09:41.864913   44369 round_trippers.go:577] Response Headers:  
E1214 13:09:41.865079   44369 memcache.go:265] couldn't get current server API group list: Get "https://192.168.49.2:8443/api?timeout=32s": dial tcp 192.168.49.2:8443: connect: no route to host  
I1214 13:09:41.865128   44369 cached_discovery.go:120] skipped caching discovery info due to Get "https://192.168.49.2:8443/api?timeout=32s": dial tcp 192.168.49.2:8443: connect: no route to host  
I1214 13:09:41.865187   44369 shortcut.go:100] Error loading discovery information: Get "https://192.168.49.2:8443/api?timeout=32s": dial tcp 192.168.49.2:8443: connect: no route to host  
I1214 13:09:41.865462   44369 round_trippers.go:466] curl -v -XGET  -H "Accept: application/json;g=apidiscovery.k8s.io;v=v2beta1;as=APIGroupDiscoveryList,application/json" -H "User-Agent: kubectl/v1.28.4 (linux/amd64) kubernetes/bae2c62" 'https://192.  
168.49.2:8443/api?timeout=32s'  
I1214 13:09:44.936925   44369 round_trippers.go:508] HTTP Trace: Dial to tcp:192.168.49.2:8443 failed: dial tcp 192.168.49.2:8443: connect: no route to host
```
### Solution
#### Verify kubernetes context
```bash
kubectl config get-contexts  
CURRENT   NAME                           CLUSTER                        AUTHINFO                                                               NAMESPACE  
         docker-desktop                 docker-desktop                 docker-desktop                                                            
*         minikube                       minikube                       minikube                                                               default
```
#### Change to docker-desktop(or other) context
```bash
kubectl config use-context docker-desktop    
Switched to context "docker-desktop".
```

## Other local Kubernetes
- [Kind](https://kind.sigs.k8s.io/)
- [MicroK8s](https://microk8s.io/)
- [Kubernetes on docker-desktop](https://docs.docker.com/desktop/kubernetes/)

## Links
- https://docs.microsoft.com/es-mx/azure/aks/kubernetes-walkthrough-portal
- https://learn.microsoft.com/es-mx/azure/aks/learn/quick-kubernetes-deploy-portal?tabs=azure-cli
- https://kubedex.com/google-gke-vs-microsoft-aks-vs-amazon-eks/
- https://k8syaml.com/

### Tools
- https://github.com/MuhammedKalkan/OpenLens
- https://github.com/alebcay/openlens-node-pod-menu
- https://github.com/andrea-falco/lens-multi-pod-logs
- https://k9scli.io/