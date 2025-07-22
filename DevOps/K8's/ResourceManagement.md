# Kubernetes – ResourceManagement

🗓️ **Date:** 22-07-2025  
🕒 **Time:** 21:57  

🏷️ **Tags:** #kubernetes #devops #ResourceManagement  

---

## 📝 Notes

##### Kubernetes Resource Management

Kubernetes **resource management** is the process of allocating, limiting, and optimizing **compute resources** (CPU, memory, and sometimes GPU or ephemeral storage) for containers running in a cluster.

It helps ensure:

- **Efficient resource utilization**
- **Fair sharing** across workloads
- **System stability** under load
- **Guaranteed QoS (Quality of Service)**

---
##### Key Concepts

###### 1. **Requests**

- The **minimum** amount of resource a container is guaranteed to get.    
- Used by the scheduler to decide **where** to place the pod.
- If not set, Kubernetes assumes **0**, and the pod may be starved.

```yaml
resources:
  requests:
    memory: "256Mi"
    cpu: "250m"
```
###### 2. **Limits**

- The **maximum** resource a container can use.
- If a container exceeds its **CPU limit**, it will be throttled.
- If it exceeds **memory limit**, it will be **terminated (OOMKilled)**.

```yaml
resources:
  limits:
    memory: "512Mi"
    cpu: "500m"
```

###### 3.**CPU & Memory Units**

- **CPU:** Expressed in millicores (`1000m = 1 core`)
    - `500m` = 0.5 vCPU
- **Memory:** Expressed in bytes
    - `128974848`, `129M`, or `123Mi` (Mi = mebibyte)        

---

##### **Quality of Service (QoS) Classes**

Kubernetes assigns one of three **QoS classes** to pods based on requests and limits:

| QoS Class      | Conditions                                                           |
| -------------- | -------------------------------------------------------------------- |
| **Guaranteed** | Requests and limits are set **and equal** for **all containers**     |
| **Burstable**  | Requests and limits are set but **not equal** or **not set for all** |
| **BestEffort** | No requests or limits are set for **any container**                  |

---

##### **Example Configuration**

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: resource-demo
spec:
  containers:
  - name: app
    image: nginx
    resources:
      requests:
        memory: "128Mi"
        cpu: "250m"
      limits:
        memory: "256Mi"
        cpu: "500m"
```

---

##### **Commands to View Resources**

###### View current usage

```bash
kubectl top pod
kubectl top node
```

###### Describe pod to check requests and limits

```bash
kubectl describe pod <pod-name>
```

###### View resource allocations

```bash
kubectl get pod <pod-name> -o jsonpath="{.spec.containers[*].resources}"
```

---

##### **Why Use Resource Management**

- Prevent noisy neighbors (pods consuming excessive resources)
- Ensure high-priority apps get required CPU/memory
- Optimize scheduling and bin packing
- Prevent cluster instability due to OOM or CPU starvation

---

## 🧾 Commands

##### Check Pod and Node Resource Usage

###### View live CPU and memory usage of pods

```bash
kubectl top pod
```

###### View CPU and memory usage of a specific pod

```bash
kubectl top pod <pod-name>
```

###### View live CPU and memory usage of nodes

```bash
kubectl top node
```

---

##### Inspect Requests and Limits

###### Get a pod’s resource requests and limits

```bash
kubectl get pod <pod-name> -o jsonpath="{.spec.containers[*].resources}"
```

###### Describe a pod to see detailed resource configuration

```bash
kubectl describe pod <pod-name>
```

---

##### Set Resource Requests and Limits (via YAML)

###### Apply a pod or deployment YAML with resource limits

```bash
kubectl apply -f pod-with-resources.yaml
```

---

##### 
##### Limit Ranges

###### List all limit ranges in a namespace

```bash
kubectl get limitrange -n <namespace>
```

###### Describe a specific limit range

```bash
kubectl describe limitrange <name> -n <namespace>
```

###### Create a limit range from a YAML file

```bash
kubectl apply -f limit-range.yaml
```

###### Delete a limit range

```bash
kubectl delete limitrange <name> -n <namespace>
```

---

##### Example: Set CPU/Memory Requests and Limits in Pod YAML

```yaml
resources:
  requests:
    memory: "128Mi"
    cpu: "250m"
  limits:
    memory: "256Mi"
    cpu: "500m"
```

---

Let me know if you'd like ready-to-use YAML templates for resource quotas, limit ranges, or deployment examples with proper resource configurations.