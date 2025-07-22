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

---

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

---

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

---

##### 3. **CPU & Memory Units**

- **CPU:** Expressed in millicores (`1000m = 1 core`)
    
    - `500m` = 0.5 vCPU
        
- **Memory:** Expressed in bytes
    
    - `128974848`, `129M`, or `123Mi` (Mi = mebibyte)
        

---

##### 4. **Quality of Service (QoS) Classes**

Kubernetes assigns one of three **QoS classes** to pods based on requests and limits:

|QoS Class|Conditions|
|---|---|
|**Guaranteed**|Requests and limits are set **and equal** for **all containers**|
|**Burstable**|Requests and limits are set but **not equal** or **not set for all**|
|**BestEffort**|No requests or limits are set for **any container**|

---

##### 5. **Example Configuration**

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

##### 6. **Commands to View Resources**

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

##### 7. **Why Use Resource Management**

- Prevent noisy neighbors (pods consuming excessive resources)
    
- Ensure high-priority apps get required CPU/memory
    
- Optimize scheduling and bin packing
    
- Prevent cluster instability due to OOM or CPU starvation
    

---

Let me know if you’d like help creating a custom resource quota, limit range, or best practices for production workloads.

## 🧾 Commands

```bash
# Example:
kubectl get pods
```
