# Kubernetes – Daemonsets

🗓️ **Date:** 2025-07-02  
🕒 **Time:** 00:06  

🏷️ **Tags:** #kubernetes #devops #Daemonsets  

---

## 📝 Notes

##### DaemonSet
A **DaemonSet** ensures that a copy of a specific pod runs on **all (or some) nodes** in a Kubernetes cluster. It's commonly used for background system-level tasks.

###### Use Cases
- Running cluster-wide agents: 
    - log collectors (e.g., Fluentd, Filebeat)
    - monitoring agents (e.g., Prometheus Node Exporter)
- Running storage daemons (e.g., Ceph, GlusterFS)
- Running networking tools (e.g., CNI plugins)

###### Key Features
- Runs **one pod per node**. 
- Automatically creates pods when:
    - a new node joins the cluster.
- Deletes pods when:
    - a node is removed from the cluster.
- Can be scoped to specific node groups using labels and selectors.

###### DaemonSet YAML Example
```yaml
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: my-daemonset
spec:
  selector:
    matchLabels:
      name: my-daemon-pod
  template:
    metadata:
      labels:
        name: my-daemon-pod
    spec:
      containers:
      - name: my-container
        image: busybox
        command: ["sh", "-c", "while true; do echo Hello from DaemonSet; sleep 30; done"]
```

###### Scheduling on Specific Nodes
Use `nodeSelector`, `nodeAffinity`, or `tolerations` to control where the DaemonSet runs.
```yaml
spec:
  template:
    spec:
      nodeSelector:
        disktype: ssd
```

---

###### Rolling Updates for DaemonSets

DaemonSets support **rolling updates** via `updateStrategy`:

```yaml
spec:
  updateStrategy:
    type: RollingUpdate
```

Other strategies:

- `OnDelete`: Pods are only updated when manually deleted.
    

---

###### Commands for Managing DaemonSets

```bash
kubectl get daemonset
kubectl describe daemonset <name>
kubectl delete daemonset <name>
kubectl rollout status daemonset <name>
```

---

###### Differences Between DaemonSet and Deployment

|Feature|DaemonSet|Deployment|
|---|---|---|
|Pod Count|One per node|User-defined replica count|
|Scheduling|All or selected nodes|Scheduler-managed|
|Use Case|Background system agents|App workloads and services|

---

Let me know if you'd like a cheat sheet or visual architecture summary.

## 🧾 Commands

**📘 Common Commands:**
```bash
kubectl get daemonsets # List all DaemonSets
kubectl describe daemonset <name> # Detailed info
kubectl delete daemonset <name> # Remove DaemonSet
```
