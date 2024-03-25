# Kubernetes hackathon

## Prerequisites
- Kubectl installation
	- https://kubernetes.io/docs/tasks/tools/
- Minikube installation
	- https://minikube.sigs.k8s.io/docs/start/
- Start the cluster using 
	 ```minikube start```
- Ensure minikube cluster context is set in **kubectl** command


## Task 1
List all kubernetes nodes using `kubectl get nodes` command and paste the output below
	    

<img width="398" alt="Screenshot 2024-03-25 at 12 36 31 PM" src="https://github.com/amilmshaji/k8s-basics-hackathon/assets/95700607/2bc2e562-ef24-4d06-ad16-84e38356fb1a">



## Task 2
describe one kubernetes node using `kubectl describe node/<node-name>` command and paste the section from the terminal output below

**Number of cpu**:     

<img width="384" alt="Screenshot 2024-03-25 at 12 43 20 PM" src="https://github.com/amilmshaji/k8s-basics-hackathon/assets/95700607/e416ca92-3de2-40a4-b9a8-6ee5971f36fe">


**Hostname**: 

<img width="282" alt="Screenshot 2024-03-25 at 12 46 10 PM" src="https://github.com/amilmshaji/k8s-basics-hackathon/assets/95700607/9b8999cc-3a94-4c16-96d0-908f4412d9f0">

**Cpu and Memory usage**:

<img width="496" alt="Screenshot 2024-03-25 at 12 49 11 PM" src="https://github.com/amilmshaji/k8s-basics-hackathon/assets/95700607/cfc190d8-2899-4db4-b03f-e30bde26f803">



## Task 3
List all kubernetes namespaces using `kubectl get namespace` command and paste the output below
	    

<img width="296" alt="Screenshot 2024-03-25 at 12 50 22 PM" src="https://github.com/amilmshaji/k8s-basics-hackathon/assets/95700607/17d5906d-1e8e-4e35-986f-8cbb86107d66">


## Task 4
List all the pods in '*kube-system*' namespace using `kubectl get pods -n <namespce command>`
	    

<img width="528" alt="Screenshot 2024-03-25 at 12 53 10 PM" src="https://github.com/amilmshaji/k8s-basics-hackathon/assets/95700607/e2ed01f4-2f3f-42e2-8b70-e391e8498afa">




## Task 5

Create a namespace with name `my-namespace` using `kubectl create namespace <namespace name>` command

List all namespaces like  in **Task 3**  and paste output below
```
➜  ~  kubectl create namespace my-namespace
namespace/my-namespace created
➜  ~ kubectl get namespace                 
NAME              STATUS   AGE
default           Active   3d1h
kube-node-lease   Active   3d1h
kube-public       Active   3d1h
kube-system       Active   3d1h
my-namespace      Active   27s

```
## Task 6

Create a Pod with name `nginx` image `nginx:latest` in `my-namespace` using yaml configurations.

1. Create yaml configuration to create a pod should run on the namespace you created in the last **Task 5**

	Refference: https://kubernetes.io/docs/concepts/workloads/pods/#using-pods
3. Use `kubectl apply -f <filename>` command to apply the configuration


Copy the yaml content below
```
apiVersion: v1
kind: Pod
metadata:
  name: nginx
  namespace: my-namespace
spec:
  containers:
  - name: nginx
    image: nginx:latest
    ports:
    - containerPort: 80
```

List all the pods in the namespace `my-namespace` using `kubectl get pods -n <namespace>` command
```
kubectl get pods -n my-namespace
NAME    READY   STATUS    RESTARTS   AGE
nginx   1/1     Running   0          10s

```

Get extra details about the pods in the namespace `my-namespace` using `kubectl get pods -n <namespace> -o wide` command
```
kubectl get pods -n my-namespace -o wide
NAME    READY   STATUS    RESTARTS   AGE   IP           NODE       NOMINATED NODE   READINESS GATES
nginx   1/1     Running   0          3m    10.244.0.5   minikube   <none>           <none>

```

## Task 7

Get details about `nginx` pod in `my-namespace`  using `kubectl describe pods/<pod-name> -n <namespace>` command


```
kubectl describe pods/nginx -n my-namespace
Name:             nginx
Namespace:        my-namespace
Priority:         0
Service Account:  default
Node:             minikube/192.168.49.2
Start Time:       Mon, 25 Mar 2024 13:24:44 +0530
Labels:           <none>
Annotations:      <none>
Status:           Running
IP:               10.244.0.5
IPs:
  IP:  10.244.0.5
Containers:
  nginx:
    Container ID:   docker://2a047df0404b8d9f9efff7b60e62ec927819e254222fa5f667f7036ebc4dcd4f
    Image:          nginx:latest
    Image ID:       docker-pullable://nginx@sha256:6db391d1c0cfb30588ba0bf72ea999404f2764febf0f1f196acd5867ac7efa7e
    Port:           80/TCP
    Host Port:      0/TCP
    State:          Running
      Started:      Mon, 25 Mar 2024 13:24:48 +0530
    Ready:          True
    Restart Count:  0
    Environment:    <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-qqzln (ro)
Conditions:
  Type              Status
  Initialized       True 
  Ready             True 
  ContainersReady   True 
  PodScheduled      True 
Volumes:
  kube-api-access-qqzln:
    Type:                    Projected (a volume that contains injected data from multiple sources)
    TokenExpirationSeconds:  3607
    ConfigMapName:           kube-root-ca.crt
    ConfigMapOptional:       <nil>
    DownwardAPI:             true
QoS Class:                   BestEffort
Node-Selectors:              <none>
Tolerations:                 node.kubernetes.io/not-ready:NoExecute op=Exists for 300s
                             node.kubernetes.io/unreachable:NoExecute op=Exists for 300s
Events:
  Type    Reason     Age    From               Message
  ----    ------     ----   ----               -------
  Normal  Scheduled  4m47s  default-scheduler  Successfully assigned my-namespace/nginx to minikube
  Normal  Pulling    4m48s  kubelet            Pulling image "nginx:latest"
  Normal  Pulled     4m44s  kubelet            Successfully pulled image "nginx:latest" in 3.279s (3.279s including waiting)
  Normal  Created    4m44s  kubelet            Created container nginx
  Normal  Started    4m44s  kubelet            Started container nginx
➜  ~ 


```

## Task 8

Port forward the `nginx` (running in namespace `my-namespace`) pods's port `80` to your local systems port `9090` using `kubectl port-forward` command 

Refference: 
https://kubernetes.io/docs/reference/kubectl/generated/kubectl_port-forward/

```
➜  ~ kubectl port-forward pod/nginx 9090:80    
Forwarding from 127.0.0.1:9090 -> 80
Forwarding from [::1]:9090 -> 80
Handling connection for 9090

```

Open another terminal  and do a curl to the port using given command
`curl http://localhost:9090`

```
➜  ~ curl http://localhost:9090
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>

```


## Task 9

List all the logs of pod `nginx` running in `my-namespace`  using `kubectl logs` command

Refference: 
https://kubernetes.io/docs/reference/kubectl/generated/kubectl_logs
```
➜  ~ kubectl logs nginx
/docker-entrypoint.sh: /docker-entrypoint.d/ is not empty, will attempt to perform configuration
/docker-entrypoint.sh: Looking for shell scripts in /docker-entrypoint.d/
/docker-entrypoint.sh: Launching /docker-entrypoint.d/10-listen-on-ipv6-by-default.sh
10-listen-on-ipv6-by-default.sh: info: Getting the checksum of /etc/nginx/conf.d/default.conf
10-listen-on-ipv6-by-default.sh: info: Enabled listen on IPv6 in /etc/nginx/conf.d/default.conf
/docker-entrypoint.sh: Sourcing /docker-entrypoint.d/15-local-resolvers.envsh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/20-envsubst-on-templates.sh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/30-tune-worker-processes.sh
/docker-entrypoint.sh: Configuration complete; ready for start up
2024/03/25 07:40:59 [notice] 1#1: using the "epoll" event method
2024/03/25 07:40:59 [notice] 1#1: nginx/1.25.4
2024/03/25 07:40:59 [notice] 1#1: built by gcc 12.2.0 (Debian 12.2.0-14) 
2024/03/25 07:40:59 [notice] 1#1: OS: Linux 6.6.12-linuxkit
2024/03/25 07:40:59 [notice] 1#1: getrlimit(RLIMIT_NOFILE): 1048576:1048576
2024/03/25 07:40:59 [notice] 1#1: start worker processes
2024/03/25 07:40:59 [notice] 1#1: start worker process 29
2024/03/25 07:40:59 [notice] 1#1: start worker process 30
2024/03/25 07:40:59 [notice] 1#1: start worker process 31
2024/03/25 07:40:59 [notice] 1#1: start worker process 32
2024/03/25 07:40:59 [notice] 1#1: start worker process 33
2024/03/25 07:40:59 [notice] 1#1: start worker process 34
2024/03/25 07:40:59 [notice] 1#1: start worker process 35
2024/03/25 07:40:59 [notice] 1#1: start worker process 36
127.0.0.1 - - [25/Mar/2024:08:45:16 +0000] "GET / HTTP/1.1" 200 615 "-" "curl/8.4.0" "-"


```

## Task 10
Delete the pod `nginx` using the yaml configurations we created in the **Task 6**

`kubectl delete -f < yaml file name with path from task 6>`

Refference:
https://kubernetes.io/docs/reference/kubectl/generated/kubectl_delete

```
kubectl delete -f "nginx_pod.yaml"                                   
pod "nginx" deleted
pod "nginx" deleted

```

Delete the namespace `my-namespace` using `kubectl delete namespace <namespace>` command

```
➜  ~ kubectl delete namespace my-namespace
namespace "my-namespace" deleted

```
