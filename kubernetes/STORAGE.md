# Storage

## Volume


## Example

```yaml
# Example of how Storage work
apiVersion: 1
kind: Pod
metadata:
  name: storage-k8s-example
specs:
  containers:

  - name: nginx
    image: nginx:alpine
    volumeMounts:
    - name: html
      mountPath: /usr/share/nginx/html
      readOnly: true

  - name: html-updater
    image: alpine
    command: ["/bin/sh", "-c"]
    args:
       # Simulate changes in html by periodically updating html file
       - while true; do date >> /html/index.html; sleep 10; done
    resources:
    volumeMounts:
    - name: html
      mountPath: /html
      readOnly: false

  volumes:
  - name: html
    emptyDir: {} # lifecycle tied to pod
```