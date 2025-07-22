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

---

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

---

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

---

##### Comparison: `nodeSelector` vs `nodeAffinity`

|Feature|`nodeSelector`|`nodeAffinity`|
|---|---|---|
|Logic support|Simple key=value|Advanced (`In`, `NotIn`, `Exists`)|
|Hard/Soft constraint|Only hard|Hard and soft|
|Expressiveness|Limited|Flexible|

---

##### Related Commands

###### Label a node

```bash
kubectl label node <node-name> disktype=ssd
```

###### View pod placement

```bash
kubectl get pod <pod-name> -o wide
```

###### Describe a pod to see affinity rules

```bash
kubectl describe pod <pod-name>
```

---

Let me know if you'd like to see an example combining **nodeAffinity** with **taints**, **tolerations**, or **anti-affinity** rules!

---

## 🧾 Commands

```bash
# Example:
kubectl get pods
```
