# Kubernetes – NodeSelector

🗓️ **Date:** 22-07-2025  
🕒 **Time:** 22:21  

🏷️ **Tags:** #kubernetes #devops #NodeSelector  

---

## 📝 Notes

##### Node Selector in Kubernetes

A **Node Selector** is the simplest way in Kubernetes to **control which node(s)** a **pod** can be scheduled on. It works by **matching pod labels to node labels**, allowing you to **pin** or **target pods to specific nodes** based on node attributes like hardware, region, environment, etc.
##### How It Works
- **Nodes** are labeled with key-value pairs (e.g., `disktype=ssd`, `env=prod`)
- **Pods** specify a `nodeSelector` section to indicate the required node labels
- The Kubernetes scheduler uses this to filter where a pod can be placed

##### Example: Labeling a Node
```bash
kubectl label nodes worker-node-1 disktype=ssd
```

##### Example: Pod with Node Selector
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod
spec:
  containers:
  - name: nginx
    image: nginx
  nodeSelector:
    disktype: ssd
```

This pod will **only be scheduled** on nodes with the label `disktype=ssd`.


##### Use Cases
- Run GPU workloads on labeled GPU nodes (`gpu=true`)    
- Separate staging vs. production pods
- Place workloads on high-memory or SSD-backed nodes
- Route workloads to specific availability zones or regions

##### Limitations
- **One-to-one matching only**: `nodeSelector` doesn’t support complex logic (like OR, NOT)    
- Doesn’t support **multi-label conditions** unless all labels match exactly
- For more flexibility, use:
    - **nodeAffinity** (preferred and required rules)
    - **taints and tolerations** (for excluding workloads)


## 🧾 Commands

##### Label Nodes (to Enable Node Selector Targeting)

###### Add a label to a node
```bash
kubectl label node <node-name> <key>=<value>
```
**Example:**
```bash
kubectl label node worker-node-1 disktype=ssd
```
###### List all nodes and their labels
```bash
kubectl get nodes --show-labels
```
###### Get labels of a specific node
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

---
##### Create Pods Using Node Selector
###### Apply a pod YAML with a nodeSelector
```bash
kubectl apply -f pod-with-node-selector.yaml
```
###### Example `pod-with-node-selector.yaml`:
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod
spec:
  containers:
  - name: nginx
    image: nginx
  nodeSelector:
    disktype: ssd
```
---
##### Verify Pod Scheduling
###### Check where the pod is scheduled
```bash
kubectl get pod nginx-pod -o wide
```
###### Describe pod to see nodeSelector applied and scheduling details
```bash
kubectl describe pod nginx-pod
```
---
