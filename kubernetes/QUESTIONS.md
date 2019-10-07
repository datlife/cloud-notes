# Common questions

### How can one access a Pod from outside of Kubernetes?

Answer: Use ` kubectl port-forward`
```bash
# Listen on 8000 locally and forward to 80 in pod
kubectl port-forward pod/[pod-name] 8000:80

# Listen on port 8080 locally and forward to deployment's pod.
kubectl port-forward deployment/[deployment-name] 8080
```

### How can you quickly test if a Service and Pod is working?

Answer: Use `kubeectl exec` to shell into a Pod/Container

```bash
# Shell into a Pod and test a URL
kubectl exec [pod-name] -- curl -s http://podIp

# Install and use cURL
kubectl exec [pod-name] -it sh
> apk add --no-cache curl
> curl -s http://podIP