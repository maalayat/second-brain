### Install
```
sudo snap install microk8s --classic
```
### Check status
```
microk8s status --wait-ready
```
```
Insufficient permissions to access MicroK8s.  
You can either try again with sudo or add the user mayalaat to the 'microk8s' group:  
  
   sudo usermod -a -G microk8s mayalaat  
   sudo chown -R mayalaat ~/.kube  
  
After this, reload the user groups either via a reboot or by running 'newgrp microk8s'
```
### Install addons
```
microk8s enable dns ingress registry storage
```
### Yon can't use kubectl directly
You should use microk8s firts
### Get nodes
```
microk8s kubectl get nodes
```
```
NAME       STATUS   ROLES    AGE    VERSION  
mayalaat   Ready    <none>   9m7s   v1.28.3
```
### Get pods
```
microk8s kubectl get pods -A
```
```
NAMESPACE            NAME                                      READY   STATUS    RESTARTS   AGE  
kube-system          calico-node-ct72x                         1/1     Running   0          13m  
kube-system          coredns-864597b5fd-cjzn4                  1/1     Running   0          13m  
kube-system          calico-kube-controllers-77bd7c5b-5sfdd    1/1     Running   0          13m  
kube-system          hostpath-provisioner-7df77bc496-tmgrg     1/1     Running   0          6m43s  
ingress              nginx-ingress-microk8s-controller-qqf7m   1/1     Running   0          6m45s  
container-registry   registry-6c9fcc695f-bktxr                 1/1     Running   0          6m42s
```
### Build image
```
docker build -t solmedia/demo-microk8s .
```
```
REPOSITORY                 TAG            IMAGE ID       CREATED         SIZE  
solmedia/demo-microk8s     latest         837fc8e35a51   27 hours ago    618MB
```
### Tagging
```
docker tag solmedia/demo-microk8s localhost:32000/demo-microk8s
```
```
docker images                                                     
REPOSITORY                      TAG            IMAGE ID       CREATED         SIZE  
localhost:32000/demo-microk8s   latest         837fc8e35a51   27 hours ago    618MB
```
### Pushing
```
docker push localhost:32000/demo-microk8s
```
### Apply
```
microk8s kubectl apply -f app-microk8s.yaml
```
```
ingress.networking.k8s.io/demo-microk8s-ingress created  
service/demo-microk8s created  
deployment.apps/demo-microk8s created
```
### Get Ingress
```
microk8s kubectl get ingress
```
```
NAME                    CLASS    HOSTS   ADDRESS     PORTS   AGE  
demo-microk8s-ingress   public   *       127.0.0.1   80      63s
```
### Get Service
```
microk8s kubectl get svc  
NAME            TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)    AGE  
kubernetes      ClusterIP   10.152.183.1     <none>        443/TCP    4h47m  
demo-microk8s   ClusterIP   10.152.183.231   <none>        8080/TCP   2m4s
```
### Get Pod
```
microk8s kubectl get pods      
NAME                             READY   STATUS    RESTARTS   AGE  
demo-microk8s-6ccc7cfc44-wnm5t   1/1     Running   0          2m34s
```

### Consume service
```
curl -X GET http://localhost/demo-microk8s/greet  
{"message":"Hello World, demo-microk8s-6ccc7cfc44-wnm5t/10.1.30.204!","greeting":null}%
```