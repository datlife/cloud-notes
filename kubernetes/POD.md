# Kubernetes Pod

## Overview
* Smallest object in K8s
* Environment for one or more containers
* Organize app into Pods (server, caching, api, database)
* Pod has an unique IP, CPU/mem available across containers.
* Pod can scale horizontally by adding `Pod replicas`.
* Live and die but  never come back to life.

##  Create Pod
### Imperative
```bash
# Run nginx container  in a Pod
kubectl run [podname] --image=nginx:alpine
```
### Declarative (`YAML`)

* YAML 101: **Indentation matters**, USE SPACE **NOT** Tabs
    ```yaml
    key: value
    complexMap:
    key1: value
    key2:
        subkey: value
    items:
    - item1
    - item2
    itemsMap:
    - map1: value
        map1Prop: value
    - map2: value
        map2Prop: value
    ```

* Create a pod using YAML: `nginx.pod.yml`
    ```yaml
    # Required fields : apiVersion, kind, metadata
    apiVersion: 1   # K8s API version
    kind: Pod       # Type of K8s resource
    metadata:       # name, UID or namespace
    name: myapp-pod
    labels:
        app: myapp
        tier: load-balancer  # allow deployment to this pod
    spec:
    containers:
    - name: myapp-container
        image: busybox
        command: ['sh', '-c', 'echo Hello World!']
    ```

* To execute:
    ```bash
    # Perform a 'trial' create and also validate the YAML
    kubectl create -f file.pod.yml --dry-run --validate=true

    # Create a Pod from YAML
    # Will error if Pod already exists
    kubectl create -f file.pod.yml

    # Create / apply changes to a Pod
    # Preferred way
    kubectl apply -f file.pod.yml

    # Save previous config
    kubectl create -f file.pod.yml --save-config
    ```

## Pod management
```bash
# List only Pods
kubectl get pods

# List all resources
kubectl get all
```

## Stop / Kill a Pod
```bash
# Will cause pod to be recreated
kubectl delete pod [name-of-pod]

# To actually delete a pod, delete deployment that manage pod
kubectl delete deployment [name-of-pod]
```

## Pod's Health

> A `Probe` is a diagnostic performed periodically by the kubelet on a container.

* Two types o `Probes`
    * **Liveness probe**: determines if a pod is healthy and running as expected
    * **Readiness probe**: determine if a pod should receive requests
* Failed Pod containers are recreated by default (`restartPolicy` default).

* Depending on application type, there are several actions we can define a probe
    * `ExecAction`: executes an action inside a container.
    * `TCPSocketAction`: TCP check against the containers' IP address on a specified port.
    * `HTTPGetAction`: a GET request against a container

### Example:

