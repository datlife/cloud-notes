# Service Demo

## Goal
* Understand how Service works in K8s.
* Understand types of Service in K8s (`ClusterIp`, `NodePort`, `LoadBalancer`)
* How Pods communicate across cluster.


## Setup
* Deploy 3 `nginx` Pods, 2 are in `nginx.deployment.yml` and 1 is a standalone.
    ```bash
    # Launch 2 pods in same deployment
    kubectl apply -f nginx.deployment.yml --replicas=2

    # Launch 1 standalone
    kubectl run nginx-standalone --image=nginx:alpine
    ```

## Procedures

* Go to any pod and ping other pod's ip using `curl http://other-pod-ip`. This approach is okay when external / internal caller knows pod's ip. However, *`Pod` in K8s is mortal*. To maintain high availability, a solution is to define a `Service` and let it handle the life-cycle of Pods. Hence, external caller does not have to worry about the availability of a particular pod.

### `ClusterIp` Service
* Create an `nginx Service`:
    ```bash
    kubectl apply -f nginx.service.yml
    ```
* Template:
    ```yml
    apiVersion: v1
    kind: Service
    metadata:
    name: nginx-clusterip  # this allows caller to do http://nginx-clusterip
    spec:
    type: ClusterIP
    selector:
        app: my-nginx  # select all pods that has my-nginx selector
    ports:
    - port: 8080
        targetPort: 80
    ```
* Now, external caller can access nginx service by calling `curl http://nginx-clusterip`

### `NodePort` Service

```yml
apiVersion: v1
kind: Service
metadata:
name: nginx-nodeport  # this allows caller to do http://nginx-clusterip
spec:
type: NodePort
selector:
    app: my-nginx  # select all pods that has my-nginx selector
ports:
- port: 8080
    targetPort: 80
    nodePort: 31000
```

* Service can be accessed `localhost:31000`

### LoadBalancer
