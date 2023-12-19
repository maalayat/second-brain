### Install
- https://k3d.io/v5.6.0/#releases

### Usage
k3d only needs docker
### Create a cluster
```bash
k3d cluster create
```
```
INFO[0000] Prep: Network                                   
INFO[0000] Created network 'k3d-k3s-default'               
INFO[0000] Created image volume k3d-k3s-default-images     
INFO[0000] Starting new tools node...                      
INFO[0001] Creating node 'k3d-k3s-default-server-0'        
INFO[0001] Pulling image 'ghcr.io/k3d-io/k3d-tools:5.6.0'    
INFO[0002] Pulling image 'docker.io/rancher/k3s:v1.27.4-k3s1'    
INFO[0004] Starting Node 'k3d-k3s-default-tools'           
INFO[0011] Creating LoadBalancer 'k3d-k3s-default-serverlb'    
INFO[0012] Pulling image 'ghcr.io/k3d-io/k3d-proxy:5.6.0'    
INFO[0019] Using the k3d-tools node to gather environment information    
INFO[0020] HostIP: using network gateway 172.20.0.1 address    
INFO[0020] Starting cluster 'k3s-default'                  
INFO[0020] Starting servers...                             
INFO[0020] Starting Node 'k3d-k3s-default-server-0'        
INFO[0024] All agents already running.                     
INFO[0024] Starting helpers...                             
INFO[0024] Starting Node 'k3d-k3s-default-serverlb'        
INFO[0030] Injecting records for hostAliases (incl. host.k3d.internal) and for 2 network members into CoreDNS configmap...    
INFO[0032] Cluster 'k3s-default' created successfully!
```
### Cluster info
```
kubectl cluster-info
```
```
Kubernetes control plane is running at https://0.0.0.0:34519  
CoreDNS is running at https://0.0.0.0:34519/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy  
Metrics-server is running at https://0.0.0.0:34519/api/v1/namespaces/kube-system/services/https:metrics-server:https/proxy
```
### Nodes
```
kubectl get nodes  
```
```
NAME                       STATUS   ROLES                  AGE   VERSION  
k3d-k3s-default-server-0   Ready    control-plane,master   13m   v1.27.4+k3s1
```
### Docker ps
```
docker ps
```
```
CONTAINER ID   IMAGE                            COMMAND                  CREATED          STATUS          PORTS                                                                 NAMES  
2e157b1533bf   ghcr.io/k3d-io/k3d-proxy:5.6.0   "/bin/sh -c nginx-pr…"   14 minutes ago   Up 14 minutes   80/tcp, 0.0.0.0:34519->6443/tcp                                       k3d-k3s-default-serverlb  
7f63af7bf584   rancher/k3s:v1.27.4-k3s1         "/bin/k3d-entrypoint…"   15 minutes ago   Up 14 minutes                                                                         k3d-k3s-default-server-0
```
### Pods
```
kubectl get pods -A
```
```
kube-system   coredns-77ccd57875-m2jxn                 1/1     Running     0          16m  
kube-system   local-path-provisioner-957fdf8bc-8v84s   1/1     Running     0          16m  
kube-system   metrics-server-648b5df564-c62x4          1/1     Running     0          16m  
kube-system   helm-install-traefik-crd-drqql           0/1     Completed   0          16m  
kube-system   svclb-traefik-47fa4ad9-mv2wg             2/2     Running     0          16m  
kube-system   helm-install-traefik-w6ncs               0/1     Completed   1          16m  
kube-system   traefik-64f55bb67d-8cwmr                 1/1     Running     0          16m
```
### Build image from helidon project
```bash
docker build -t solmedia/demo-k3d .
```
### Publish manifest file
```bash
kubectl apply -f app-k3d.yaml
```
```
ingress.networking.k8s.io/demo-k3d-ingress created  
service/demo-k3d created  
deployment.apps/demo-k3d created
```
### Get services
```bash
kubectl get svc  
```
```
NAME         TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)    AGE  
kubernetes   ClusterIP   10.43.0.1      <none>        443/TCP    25m  
demo-k3d     ClusterIP   10.43.27.130   <none>        8080/TCP   7s
```
### Get ingress
```bash
kubectl get ingress
```
```
NAME               CLASS     HOSTS   ADDRESS      PORTS   AGE  
demo-k3d-ingress   traefik   *       172.20.0.2   80      21s
```
### Import image
```bash
k3d image import solmedia/demo-k3d
```
```
INFO[0000] Importing image(s) into cluster 'k3s-default'    
INFO[0000] Starting new tools node...                      
INFO[0000] Starting Node 'k3d-k3s-default-tools'           
INFO[0000] Saving 1 image(s) from runtime...               
k3d image import solmedia/demo-k3dINFO[0011] Importing images into nodes...                  
INFO[0011] Importing images from tarball '/k3d/images/k3d-k3s-default-images-20231218160331.tar' into node 'k3d-k3s-default-server-0'...    
INFO[0031] Removing the tarball(s) from image volume...    
INFO[0032] Removing k3d-tools node...                      
INFO[0033] Successfully imported image(s)                  
INFO[0033] Successfully imported 1 image(s) into 1 cluster(s)
```
### Get pods
```bash
kubectl get pods
```
### Port forward
```bash
kubectl port-forward svc/demo-k3d 8085:8080
```
```
Forwarding from 127.0.0.1:8085 -> 8080  
Forwarding from [::1]:8085 -> 8080
```
#### Consume service
```bash
curl -X GET http://localhost:8085/greet
```
```json
{"message":"Hello World, demo-k3d-67b5695cfc-jrqkm/10.42.0.9!","greeting":null}
```
This method only works in local test, don't in real environments
### Ingress
#### Delete cluster
```bash
k3d cluster delete
```
```
INFO[0000] Deleting cluster 'k3s-default'               
INFO[0002] Deleting cluster network 'k3d-k3s-default'   
INFO[0002] Deleting 1 attached volumes...               
INFO[0002] Removing cluster details from default kubeconfig... 
INFO[0002] Removing standalone kubeconfig file (if there is one)... 
INFO[0002] Successfully deleted cluster k3s-default!
```
#### Create a cluster with load balancer
```bash
k3d cluster create --api-port 6550 -p "8081:80@loadbalancer" --agents 2
```
```
INFO[0000] portmapping '8081:80' targets the loadbalancer: defaulting to [servers:*:proxy agents:*:proxy]    
INFO[0000] Prep: Network                                   
INFO[0000] Created network 'k3d-k3s-default'               
INFO[0000] Created image volume k3d-k3s-default-images     
INFO[0000] Starting new tools node...                      
INFO[0000] Starting Node 'k3d-k3s-default-tools'           
INFO[0001] Creating node 'k3d-k3s-default-server-0'        
INFO[0001] Creating node 'k3d-k3s-default-agent-0'         
INFO[0001] Creating node 'k3d-k3s-default-agent-1'         
INFO[0001] Creating LoadBalancer 'k3d-k3s-default-serverlb'    
INFO[0001] Using the k3d-tools node to gather environment information    
INFO[0001] HostIP: using network gateway 172.21.0.1 address    
INFO[0001] Starting cluster 'k3s-default'                  
INFO[0001] Starting servers...                             
INFO[0001] Starting Node 'k3d-k3s-default-server-0'        
INFO[0008] Starting agents...                              
INFO[0008] Starting Node 'k3d-k3s-default-agent-0'         
INFO[0008] Starting Node 'k3d-k3s-default-agent-1'         
INFO[0013] Starting helpers...                             
INFO[0013] Starting Node 'k3d-k3s-default-serverlb'        
INFO[0020] Injecting records for hostAliases (incl. host.k3d.internal) and for 4 network members into CoreDNS configmap...    
INFO[0022] Cluster 'k3s-default' created successfully!
```
#### Import image
```
k3d image import solmedia/demo-k3d
```
#### Removing nginx annotations
By using traefik
```
nginx.ingress.kubernetes.io/rewrite-target: /$2
```
#### Migration Traefik V1 to V2
- https://doc.traefik.io/traefik/migration/v1-to-v2/
#### Republish manifest file
```bash
kubectl apply -f app.yaml
```
```
kubectl apply -f app-k3d.yaml         
middleware.traefik.containo.us/demo-k3d-stripprefix created  
ingressroute.traefik.containo.us/demo-k3d-ingress-route created  
service/demo-k3d unchanged  
deployment.apps/demo-k3d created
```
#### Consumer service
```bash
curl -X GET http://localhost:8081/demo-k3d/greet
```
```json
{"message":"Hello World, demo-k3d-67b5695cfc-fbx99/10.42.0.7!","greeting":null}%
``` 

### Links
- https://traefik.io/traefik/
- https://doc.traefik.io/traefik/middlewares/http/stripprefix/
- https://doc.traefik.io/traefik/migration/v1-to-v2/
- https://github.com/traefik/traefik-migration-tool
- https://k3d.io/v5.6.0/usage/exposing_services/