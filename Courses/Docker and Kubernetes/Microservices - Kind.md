### Install
https://kind.sigs.k8s.io/docs/user/quick-start/#installing-from-release-binaries

### Usage
Kind only needs docker
### Create a cluster
```bash
kind create cluster
```
```
 Creating cluster "kind" ...  
✓ Ensuring node image (kindest/node:v1.27.3)     
✓ Preparing nodes      
✓ Writing configuration     
✓ Starting control-plane     
✓ Installing CNI     
✓ Installing StorageClass     
Set kubectl context to "kind-kind"  
You can now use your cluster with:  
  
kubectl cluster-info --context kind-kind  
  
Have a nice day! f
```
This cluster has configured a kubernetes context, in this case, kind-kind context.

### Build image from helidon project
```bash
docker build -t solmedia/demo-kind .
```
### Publish manifest file
```bash
kubectl apply -f app.yaml                                                                                        
```
### Get service
```bash
kubectl get svc        
```
```
NAME         TYPE           CLUSTER-IP      EXTERNAL-IP   PORT(S)        AGE  
demo-kind    LoadBalancer   10.96.176.112   <pending>     80:30266/TCP   26s  
kubernetes   ClusterIP      10.96.0.1       <none>        443/TCP        11m
```
Kind does not have a LoadBalancer, so there is no External-IP
### Get pods
```bash
kubectl get pods
```
```
NAME                         READY   STATUS              RESTARTS   AGE  
demo-kind-84df76f6c7-rwknw   0/1     ErrImageNeverPull   0          2m4s
```
Similar minikube problem, image can not see docker image
### Load image
```bash
kind load docker-image solmedia/demo-kind solmedia/demo-kind
```
```
Image: "solmedia/demo-kind" with ID "sha256:837fc8e35a51dd0e4705c97ce6fac6f7d8298c00c88f3a6e20c9790bd382e6e8" not yet present on node "kind-control-plane", loading...
```
### Verify pods
```bash
kubectl get pods
```
```
NAME                         READY   STATUS    RESTARTS   AGE  
demo-kind-84df76f6c7-rwknw   1/1     Running   0          9m16s
```
### Port forward
```bash
kubectl port-forward svc/demo-kind 8085:80
```
```
Forwarding from 127.0.0.1:8085 -> 8080  
Forwarding from [::1]:8085 -> 8080
```
#### Consume service
```bash
curl -X GET http://192.168.49.2:32467/greet
```
```json
{"message":"Hello World, demo-kind-84df76f6c7-rwknw/10.244.0.5!","greeting":null}%
```
This method only works in local test, don't in real environments
### Ingress
#### Delete cluster
```bash
kind delete cluster --name kind
```
```
Deleting cluster "kind" ...  
Deleted nodes: ["kind-control-plane"]
```
#### Creating a cluster with ingress support
```bash
cat <<EOF | kind create cluster --config=-
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
nodes:
- role: control-plane
  kubeadmConfigPatches:
  - |
    kind: InitConfiguration
    nodeRegistration:
      kubeletExtraArgs:
        node-labels: "ingress-ready=true"
  extraPortMappings:
  - containerPort: 80
    hostPort: 80
    protocol: TCP
  - containerPort: 443
    hostPort: 443
    protocol: TCP
EOF
```
#### Load image again
```bash
kind load docker-image solmedia/demo-kind solmedia/demo-kind
```
#### Creating NGINX server
```bash
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/main/deploy/static/provider/kind/deploy.yaml
```
```
namespace/ingress-nginx created  
serviceaccount/ingress-nginx created  
serviceaccount/ingress-nginx-admission created  
role.rbac.authorization.k8s.io/ingress-nginx created  
role.rbac.authorization.k8s.io/ingress-nginx-admission created  
clusterrole.rbac.authorization.k8s.io/ingress-nginx created  
clusterrole.rbac.authorization.k8s.io/ingress-nginx-admission created  
rolebinding.rbac.authorization.k8s.io/ingress-nginx created  
rolebinding.rbac.authorization.k8s.io/ingress-nginx-admission created  
clusterrolebinding.rbac.authorization.k8s.io/ingress-nginx created  
clusterrolebinding.rbac.authorization.k8s.io/ingress-nginx-admission created  
configmap/ingress-nginx-controller created  
service/ingress-nginx-controller created  
service/ingress-nginx-controller-admission created  
deployment.apps/ingress-nginx-controller created  
job.batch/ingress-nginx-admission-create created  
job.batch/ingress-nginx-admission-patch created  
ingressclass.networking.k8s.io/nginx created  
networkpolicy.networking.k8s.io/ingress-nginx-admission created  
validatingwebhookconfiguration.admissionregistration.k8s.io/ingress-nginx-admission created
```
#### Verifying ingress server
```bash
kubectl wait --namespace ingress-nginx \
  --for=condition=ready pod \
  --selector=app.kubernetes.io/component=controller \
  --timeout=90s
```
```
pod/ingress-nginx-controller-d9b6d7bf5-78q4w condition met
```
#### Modifying my deployment plan
```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: demo-kind-ingress
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /$2
spec:
  rules:
  - http:
      paths:
      - pathType: Prefix
        path: /demo-kind
        backend:
          service:
            name: demo-kind
            port:
              number: 8080
```
Change path, service name and port number
#### Republish manifest file
```bash
kubectl apply -f app.yaml                                                                                        
```
#### View pods
```bash
kubectl get pods
```
#### Consumer service
```bash
curl -X GET http://localhost/demo-kind/greet 
```
```
Endpoint not found%
```
#### Nginx Rewrite
```
annotations:
  nginx.ingress.kubernetes.io/rewrite-target: /$2
```
```
path: /demo-kind(/|$)(.*)
```
#### Republish manifest file
```bash
kubectl apply -f app.yaml                                                                                        
```
```
Warning: path /demo-kind(/|$)(.*) cannot be used with pathType Prefix  
ingress.networking.k8s.io/demo-kind-ingress configured  
service/demo-kind unchanged  
deployment.apps/demo-kind unchanged
```
#### Consumer service
```bash
curl -X GET http://localhost/demo-kind/greet 
```
```json
{"message":"Hello World, demo-kind-84df76f6c7-cqpdq/10.244.0.8!","greeting":null}%
```

### Links
- https://kind.sigs.k8s.io/