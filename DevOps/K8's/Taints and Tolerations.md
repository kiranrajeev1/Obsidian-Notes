# Kubernetes – Taints and Tolerations

🗓️ **Date:** 22-07-2025  
🕒 **Time:** 22:43  

🏷️ **Tags:** #kubernetes #devops #Taints #Tolerations  

---

## 📝 Notes

##### Taints and Tolerations in Kubernetes
**Taints and Tolerations** are Kubernetes mechanisms used to **control which pods can be scheduled on which nodes**, by **repelling** pods from nodes unless the pod explicitly tolerates the taint. They work as **filters**, helping enforce node isolation for specific workloads (e.g., critical apps, GPU nodes, dedicated nodes).

##### How It Works
- **Taint**: Applied to a **node**, it repels pods unless they tolerate it.
- **Toleration**: Applied to a **pod**, it allows the pod to be scheduled on a node with a matching taint.

##### Taint Format
```bash
kubectl taint nodes <node-name> <key>=<value>:<effect>
```
- `key=value`: The label key-value pair.
- `effect`: Can be one of:
    - `NoSchedule`: Pods won’t be scheduled unless they tolerate the taint.
    - `PreferNoSchedule`: Scheduler avoids placing pods but can do so if needed.
    - `NoExecute`: Evicts existing pods that don’t tolerate the taint.

**Example:**
```bash
kubectl taint nodes node1 env=prod:NoSchedule
```

##### Toleration in Pod Spec
To allow a pod to run on a tainted node, add a matching toleration in the pod manifest:
```yaml
tolerations:
- key: "env"
  operator: "Equal"
  value: "prod"
  effect: "NoSchedule"
```

##### Example: Full Pod with Toleration
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: tolerate-prod
spec:
  containers:
  - name: nginx
    image: nginx
  tolerations:
  - key: "env"
    operator: "Equal"
    value: "prod"
    effect: "NoSchedule"
```

##### Common Use Cases
- Run system or privileged workloads on dedicated nodes.
- Separate dev/staging and production workloads.
- Protect GPU nodes from general-purpose pods.
- Evict unwanted pods using `NoExecute`.

---
## 🧾 Commands

###### Add a taint to a node
```bash
kubectl taint nodes <node-name> <key>=<value>:<effect>
```
**Example:**
```bash
kubectl taint nodes node1 env=prod:NoSchedule
```
###### Remove a taint from a node
```bash
kubectl taint nodes <node-name> <key>:<effect>-
```
**Example:**
```bash
kubectl taint nodes node1 env:NoSchedule-
```
###### View taints on a node
```bash
kubectl describe node <node-name>
```
###### Apply a pod manifest with a toleration
```bash
kubectl apply -f pod-with-toleration.yaml
```
###### Example pod-with-toleration.yaml
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: tolerate-prod
spec:
  containers:
  - name: nginx
    image: nginx
  tolerations:
  - key: "env"
    operator: "Equal"
    value: "prod"
    effect: "NoSchedule"
```

