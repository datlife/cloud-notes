# Kubernetes
<center><img src=./k8s_def.png style="max-width: 720px;"></center>

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
* **Service Discovery / Load balancing**
    * Group of machine working together
* **Storage Orchestration**
* **Automate Roll-outs/Rollbacks** : Without bringing down the running app
* **Self-healing**: Automate bringing up container
* **Secret/Config Management**: Store secrets / config maps (KV pair)
* **Horizontal Scaling**: scale out 1 busy container if host is still having enough resources.
* And more.

## How

> Kubernetes provides a "declarative" way to define a cluster's state.

## Example
