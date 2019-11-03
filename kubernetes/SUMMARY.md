# Kubernetes
<center><img src=./imgs/k8s_def.png style="max-width: 720px;"></center>

## Overview
- Automate deployment, scaling, and management for containerized apps.
- Open-sourced by Google in 2014 (known as K8s).

## Why
### Problem:
- Existing management system: 
    ```bash
                    ------- Container A (VM)
                    |
    Load Balancer --------- Container B (Physical Server)
                    |
                    ------- Container C (Container)
    ```
    * Issue:
        - Hard to recover manually if one or more containers is down.

- Docker-compose:
    - Suitable for one node (local computer).
    - Hard to handle across multiple servers (networking/storage).

### K8s Key Features:

> Kubernetes provides a *framework* to run distributed systems **resiliently**.

* **Service Discovery / Load balancing**
    * Expose a container the DNS name / their own IP address.
    * If traffic in 1 container is high -> load balance and distribute traffic.

* **Storage Orchestration**
    * Automate mount a storage system (local, public cloud, etc.)
* **Automate Roll-outs/Rollbacks**:
    * Describe a desired state for your deployed containers. K8s updates/changes
    from actual state to desired state.
    * Ex: automate container upgrade / downgrade without down time.
* **Self-healing**:
    * Restart failed containers / Kill unresponsive containers through health-check.
    * Doesnt' advertise them to clients until in ready state.

* **Automatic bin packing**:
    * Specify how much CPU / Mem each container needs.
    * K8s can make better decisions to manage resources for containers.
* **Secret/Config Management**: 
    * Store and manage sensitive information (password, ssh/oauth keys).
    * Deploy / update secrets without rebuilding container images.
* **Horizontal Scaling**: scale out 1 busy container if host is still having enough resources.
* And more.

### What K8s is not:
    * K8s is not a mere orch. system.
    * Does not deploy source code and build our app. That's CI/CD task.
    * Does not provide app-level service (Spark, MySQL, caches).
    * Does not dictate logging, monitoring, alerting solutions.
    * 
Ref: Official docs ["What is Kubenetes?"](https://kubernetes.io/docs/concepts/overview/what-is-kubernetes/)

### My takes:
* Allow to maximize / utilize available computing resources at minimal cost.

## How

> Kubernetes provides a "declarative" way to define a cluster's state.

### Master Component
* Provides cluster's control plane. Make global decisions about the cluster (scheduling), and detect/response to cluster events (start a new pod).
<center><img src=./imgs/k8s_master.png style="max-width: 720px;"></center>

* **Store (etcd)**: a consistent and highly-available KV store for K8s' backing store for all cluster data.
* **kube-apiserver**: Expose Kubernetes API (YAML/Json)
* **kube-scheduler**: Decide which node to put newly created pods on. The decision is based on:
    * Individual and collective resources requirements.
    * Hardware/software/policy constraint.
    * Affinity and anti-affinity specs.
    * Data locality and inter-workload interference and deadlines.
* **kube-controller-manager**: A collection of controllers in K8s
    * `Node ctl`: notice and response when nodes go down.
    * `Replication ctl`: Maintain correct # of nodes for every replication obj.
    * `Endpoint Ctl`: populates endpoint objects (join Services and Pods).
    * `Service Account & Token ctl`: create default accounts and API access tokens for new namespaces.

### Node Components
<center><img src=./imgs/k8s_pod.png style="max-width: 720px;"></center>

* **Kubelet**: an agent runs on each node. It makes sure containers are running in a pod.
* **Kube-proxy**: 
    * A network proxy runs on each node, implementing *part* of Kubernetes Service concept.
    * Maintain network rules on nodes. Use system packet filtering layer or forward the traffic itself.
* **Container Runtime**: 
    * A instance of software responsible for running containers.
    * Can be `Docker`, `containerd`, `cri-o`, `rklet` or any that meets K8s CRI.

### Add-ons
* DNS
* Web UI (Dashboard)
* Container Resource Monitoring
* Cluster-level Logging

## Example: Run locally with Web UI
* Install Docker Desktop on Mac. Enable Kubernetes feature.
* Follow https://kubernetes.io/docs/tasks/access-application-cluster/web-ui-dashboard/
```bash
# From terminal

# 1. Create a pod from YAML file
kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.0.0-beta4/aio/deploy/recommended.yaml
  
# 2. Set up token to login
kubectl describe secret -n secret

# 3. Set up proxy to forward cluster to localhost
kubectl proxy

# Web UI should be available at 
http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/

```
