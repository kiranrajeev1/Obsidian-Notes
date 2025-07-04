# Kubernetes – Daemonsets

🗓️ **Date:** 2025-07-02  
🕒 **Time:** 00:06  

🏷️ **Tags:** #kubernetes #devops #Daemonsets  

---

## 📝 Notes

##### DaemonSet

A **DaemonSet** ensures that **a copy of a Pod runs on every node** (or
specific nodes) in the cluster
###### Use Cases:

- Running **node-level services**, such as:
- Log collectors (e.g., Fluentd, Filebeat)
- Monitoring agents (e.g., Prometheus Node Exporter)
- Networking plugins (e.g., CNI plugins)
- Security tools (e.g., antivirus, audit)

##### Basic YAML Example:

```YAML
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: my-daemon
spec:
  selector:
    matchLabels:
      name: my-daemon
  template:
    metadata:
      labels:
        name: my-daemon
    spec:
      containers:
      - name: my-container
        image: busybox

```

###### Key Features:

- **Runs on all nodes automatically** (including new ones).
- Use **node selectors or taints/tolerations** to control where Pods
  run.
- No need to specify replicas -- it's managed per node.

###### My code

```YAML
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: nginx-daemonset
  labels:
    app: nginx
spec:
  selector:
    matchLabels:
      name: nginx
  template:
    metadata:
      labels:
        name: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.25
        ports:
        - containerPort: 80
```

## 🧾 Commands

**📘 Common Commands:**
```bash
kubectl get daemonsets # List all DaemonSets
kubectl describe daemonset <name> # Detailed info
kubectl delete daemonset <name> # Remove DaemonSet
```
