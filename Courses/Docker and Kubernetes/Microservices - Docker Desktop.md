### Verify kubernetes context
```
kubectl config get-contexts  
```
```
CURRENT   NAME                           CLUSTER                        AUTHINFO                                                               NAMESPACE  
         docker-desktop                 docker-desktop                 docker-desktop                                                            
*         minikube                       minikube                       minikube                                                               default
```
#### Change to docker-desktop(or other) context
```
kubectl config use-context docker-desktop
```
```
Switched to context "docker-desktop".
```
### Build image from helidon project
```
docker build -t solmedia/demo-docker-desktop .
```
### Publish manifest file
```
kubectl apply -f app.yaml                                                                                        
```
```
service/demo-minikube created
deployment.apps/demo-minikube created
```
### Get service
```
kubectl get svc        
```
```
NAME                  TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)           AGE  
demo-docker-desktop   NodePort    10.108.115.107   <none>        38080:31526/TCP   19m  
kubernetes            ClusterIP   10.96.0.1        <none>        443/TCP           142d
```

### Trying to connect to the microservice using NodePort 38080
```
curl -X GET http://localhost:38080/greet    
```
```
curl: (7) Failed to connect to localhost port 38080 after 0 ms: Connection refused
```
### Using vpnkit by docker-desktop
#### Modifying app.yml
- NodePort to LoadBalancer
- 38080 to 80 port
### Republish manifest file
```
kubectl apply -f app.yaml
```
```
service/demo-docker-desktop configured  
deployment.apps/demo-docker-desktop unchanged
```
### Getting service
```
kubectl get svc  
```
```
NAME                  TYPE           CLUSTER-IP       EXTERNAL-IP   PORT(S)        AGE  
demo-docker-desktop   LoadBalancer   10.108.115.107   localhost     80:31526/TCP   28m  
kubernetes            ClusterIP      10.96.0.1        <none>        443/TCP        142d
```
### Access with docker desktop vpnkit using 80 port
```
curl -X GET http://localhost/greet
```
```
{"message":"Hello World, demo-docker-desktop-5c4bfbf7b4-79nsh/10.1.0.18!","greeting":null}%
```
### Links
- https://www.docker.com/blog/how-docker-desktop-networking-works-under-the-hood/
