## Helidon, docker

### Build image from helidon project
```bash
docker build -t solmedia/demo-minikube .
```
### View image created
```bash
docker images
```
```
REPOSITORY               TAG       IMAGE ID       CREATED         SIZE
solmedia/demo-minikube   latest    9b136c5fa280   3 minutes ago   618MB
```
### Run image
```
docker run -p 8080:8080 solmedia/demo-minukube
```
### Kubernetes
```bash
minikube staus
```
```
🎉  minikube 1.32.0 is available! Download it: https://github.com/kubernetes/minikube/releases/tag/v1.32.0
💡  To disable this notice, run: 'minikube config set WantUpdateNotification false'

minikube
type: Control Plane
host: Running
kubelet: Running
apiserver: Running
kubeconfig: Configured
```
### Verify nodes
```bash
kubectl get nodes
```
```
NAME       STATUS   ROLES           AGE   VERSION
minikube   Ready    control-plane   30h   v1.25.3
```
### Publish manifest file
```bash
kubectl apply -f app.yaml
```
```
service/demo-minikube created
deployment.apps/demo-minikube created
```
### Verifying if service was created
```bash
kubectl get svc
```
```
NAME            TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)    AGE
demo-minikube   ClusterIP   10.111.167.202   <none>        8080/TCP   102s
kubernetes      ClusterIP   10.96.0.1        <none>        443/TCP    30h
```
### Get pods
```bash
kubectl get pods
```
```
NAME                             READY   STATUS              RESTARTS   AGE
demo-minikube-759c699966-dbtx2   0/1     ErrImageNeverPull   0          12s
demo-minikube-c7ffdbdb7-5bwnc    0/1     ImagePullBackOff    0          10m
```
### Get logs
```
kubectl logs demo-minikube-c7ffdbdb7-5bwnc  
Error from server (BadRequest): container "demo-minikube" in pod "demo-minikube-c7ffdbdb7-5bwnc" is waiting to start: trying and failing to pull image
```
### Fixing failing to pull image error
### Get minikube env
```
 minikube docker-env
export DOCKER_TLS_VERIFY="1"
export DOCKER_HOST="tcp://192.168.49.2:2376"
export DOCKER_CERT_PATH="/home/mayalaat/.minikube/certs"
export MINIKUBE_ACTIVE_DOCKERD="minikube"

# To point your shell to minikube's docker-daemon, run:
# eval $(minikube -p minikube docker-env)
```
### To point your shell to minikube's docker-daemon
```
eval $(minikube -p minikube docker-env)
```
### View other docker images
```
 docker images    
REPOSITORY                                      TAG       IMAGE ID       CREATED         SIZE
registry.k8s.io/kube-apiserver                  v1.25.3   0346dbd74bcb   14 months ago   128MB
registry.k8s.io/kube-scheduler                  v1.25.3   6d23ec0e8b87   14 months ago   50.6MB
registry.k8s.io/kube-controller-manager         v1.25.3   603999231275   14 months ago   117MB
```
### Rebuild docker image
```bash
docker build -t solmedia/demo-minikube .
```
### Minikube images and solmedia/demo-minikube
```bash
docker images                           
```
```
REPOSITORY                                      TAG       IMAGE ID       CREATED          SIZE
solmedia/demo-minikube                          latest    9b80a98f6afc   29 seconds ago   618MB
registry.k8s.io/kube-apiserver                  v1.25.3   0346dbd74bcb   14 months ago    128MB
registry.k8s.io/kube-scheduler                  v1.25.3   6d23ec0e8b87   14 months ago    50.6MB

```
### Republish manifest file
```bash
kubectl apply -f app.yaml
```
```
service/demo-minikube created
deployment.apps/demo-minikube created
```
### Get service
```
NAME            TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)    AGE
demo-minikube   ClusterIP   10.99.87.137   <none>        8080/TCP   3m57s
```
### Access from external, trying to expose the url
```
minikube service demo-minikube --url
😿  service default/demo-minikube has no node port
```
### Change ClusterIp to NodePort
```
spec:  
type: NodePort
```
### Republish manifest file
```bash
kubectl apply -f app.yaml
```
```
service/demo-minikube configured
deployment.apps/demo-minikube unchanged
```
### Expose url again
```
minikube service demo-minikube --url
http://192.168.49.2:30115
```
### Querying service from kubernetes
```bash
curl -X GET http://192.168.49.2:30115/greet
```
```json
{"message":"Hello World!","greeting":null}
```
### Development cycle
#### Change source code
```java
final var ip = InetAddress.getLocalHost();  
return createResponse("World, "+ip);
```
#### Rebuild image
```bash
docker build -t solmedia/demo-minikube .
```
#### Delete manifest
```bash
kubectl delete -f app.yaml
```
```
service "demo-minikube" deleted
deployment.apps "demo-minikube" deleted
```
#### Republish manifest
```
kubectl apply -f app.yaml 
service/demo-minikube created
deployment.apps/demo-minikube created
```
### Check if the service is active
```
kubectl get pods -o wide
NAME                             READY   STATUS    RESTARTS   AGE   IP           NODE       NOMINATED NODE   READINESS GATES
demo-minikube-7b6c8d565c-j5zlb   1/1     Running   0          64s   172.17.0.6   minikube   <none>           <none>
```
#### Expose url
```
minikube service demo-minikube --url
http://192.168.49.2:32467
```
#### Consume service
```
curl -X GET http://192.168.49.2:32467/greet
{"message":"Hello World, demo-minikube-7b6c8d565c-j5zlb/172.17.0.6!","greeting":null}%    
```
### Minikube dashboard
```
 minikube dashboard
```

### Links
- https://minikube.sigs.k8s.io/docs/