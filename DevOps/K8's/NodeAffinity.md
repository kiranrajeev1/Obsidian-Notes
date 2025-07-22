# Kubernetes – NodeAffinity

🗓️ **Date:** 22-07-2025  
🕒 **Time:** 22:32  

🏷️ **Tags:** #kubernetes #devops #NodeAffinity  

---

## 📝 Notes

##### Node Affinity in Kubernetes
**Node Affinity** is a more powerful and flexible way to control **which nodes a pod can be scheduled on**, compared to basic `nodeSelector`. It allows you to define **rules based on node labels** using logical operators like `In`, `NotIn`, `Exists`, and more.

Node affinity is part of **pod scheduling**, helping the Kubernetes scheduler decide **where** to place pods.

##### Why Use Node Affinity?
- More expressive than `nodeSelector`
- Can define **required** or **preferred** rules
- Useful for scheduling pods based on:
    - Region/zone
    - Hardware (e.g., GPU)
    - Environment (e.g., prod, dev)
    - Custom labels

##### Types of Node Affinity
There are two main types of node affinity:

| Type                                              | Required? | Behavior                                                         |
| ------------------------------------------------- | --------- | ---------------------------------------------------------------- |
| `requiredDuringSchedulingIgnoredDuringExecution`  | Yes       | **Hard constraint**; pod won't be scheduled unless rule matches  |
| `preferredDuringSchedulingIgnoredDuringExecution` | No        | **Soft constraint**; scheduler tries to match, but not mandatory |

##### Example: Required Node Affinity (Hard Rule)
```yaml
affinity:
  nodeAffinity:
    requiredDuringSchedulingIgnoredDuringExecution:
      nodeSelectorTerms:
      - matchExpressions:
        - key: disktype
          operator: In
          values:
          - ssd
```
This ensures the pod is **only scheduled** on nodes labeled `disktype=ssd`.

##### Example: Preferred Node Affinity (Soft Rule)
```yaml
affinity:
  nodeAffinity:
    preferredDuringSchedulingIgnoredDuringExecution:
    - weight: 1
      preference:
        matchExpressions:
        - key: disktype
          operator: In
          values:
          - ssd
```

This prefers scheduling on `disktype=ssd` nodes but allows other nodes if none match.

##### Complete Pod Spec with Node Affinity
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: affinity-demo
spec:
  affinity:
    nodeAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
        nodeSelectorTerms:
        - matchExpressions:
          - key: disktype
            operator: In
            values:
            - ssd
  containers:
  - name: nginx
    image: nginx
```


##### Comparison: `nodeSelector` vs `nodeAffinity`

| Feature              | `nodeSelector`   | `nodeAffinity`                     |
| -------------------- | ---------------- | ---------------------------------- |
| Logic support        | Simple key=value | Advanced (`In`, `NotIn`, `Exists`) |
| Hard/Soft constraint | Only hard        | Hard and soft                      |
| Expressiveness       | Limited          | Flexible                           |

---
## 🧾 Commands

##### Label Nodes for Affinity Targeting
###### Add a label to a node (for affinity matching)
```bash
kubectl label node <node-name> <key>=<value>
```
**Example:**
```bash
kubectl label node worker-node-1 disktype=ssd
```
###### View all node labels
```bash
kubectl get nodes --show-labels
```
###### View labels of a specific node
```bash
kubectl get node <node-name> --show-labels
```
###### Remove a label from a node
```bash
kubectl label node <node-name> <key>-
```
**Example:**
```bash
kubectl label node worker-node-1 disktype-
```
##### Create Pods with Node Affinity

###### Apply a pod spec that uses nodeAffinity

```bash
kubectl apply -f pod-with-node-affinity.yaml
```

###### Example pod-with-node-affinity.yaml

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: affinity-demo
spec:
  affinity:
    nodeAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
        nodeSelectorTerms:
        - matchExpressions:
          - key: disktype
            operator: In
            values:
            - ssd
  containers:
  - name: nginx
    image: nginx
```

---

##### Check and Troubleshoot Affinity Behavior

###### View where the pod was scheduled

```bash
kubectl get pod affinity-demo -o wide
```

###### Describe the pod to view applied affinity rules

```bash
kubectl describe pod affinity-demo
```

###### Check why a pod is not scheduled (e.g., no matching node)

```bash
kubectl describe pod <pod-name>
```

---

Let me know if you'd like a YAML example for **preferred affinity** or want to combine it with **taints and tolerations** or **anti-affinity rules**.