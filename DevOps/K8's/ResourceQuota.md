# Kubernetes – ResourceQuota

🗓️ **Date:** 22-07-2025  
🕒 **Time:** 22:10  

🏷️ **Tags:** #kubernetes #devops #ResourceQuota  

---

## 📝 Notes

##### Resource Quotas in Kubernetes

A **ResourceQuota** in Kubernetes is used to **limit the total resource consumption** (CPU, memory, number of objects, etc.) across a namespace. This is useful for **multi-tenant clusters** or to **enforce fair usage** and **prevent overconsumption** of shared cluster resources.

##### Why Use Resource Quotas?

- Prevent a single team/app from consuming all resources in a namespace
- Ensure fair resource sharing across teams or projects
- Set hard limits on:
    - CPU and memory
    - Number of pods, PVCs, secrets, etc.        
    - LoadBalancer services, ephemeral storage, etc.

---

##### How Resource Quotas Work

- Resource quotas are **namespace-scoped**  
- They apply to all resources in that namespace
- Pods must request resources (`requests` or `limits`) for quotas to apply
- Combined with **LimitRanges**, you can enforce default resource limits

---

##### Example ResourceQuota YAML

```yaml
apiVersion: v1
kind: ResourceQuota
metadata:
  name: dev-quota
  namespace: dev
spec:
  hard:
    requests.cpu: "2"
    requests.memory: "4Gi"
    limits.cpu: "4"
    limits.memory: "8Gi"
    pods: "10"
    persistentvolumeclaims: "5"
    services: "5"
    configmaps: "10"
    secrets: "10"
```

This means:

- Max total CPU requested across all pods: 2 cores
- Max memory limit across all pods: 8Gi
- Max 10 pods, 5 PVCs, etc. in the namespace

---
##### What Happens When Limits Are Exceeded?

- If you try to create more resources (e.g., a new pod) that would exceed the quota, Kubernetes will **reject the request** with an error:    
    ```
    Error from server (Forbidden): exceeded quota: dev-quota
    ```


---

##### Common ResourceQuota Fields

| Field                    | Description                                 |
| ------------------------ | ------------------------------------------- |
| `pods`                   | Max number of pods                          |
| `services`               | Max number of services                      |
| `secrets`                | Max number of secrets                       |
| `configmaps`             | Max number of configmaps                    |
| `persistentvolumeclaims` | Max number of PVCs                          |
| `requests.cpu`           | Total CPU requests allowed (e.g., "2")      |
| `requests.memory`        | Total memory requests allowed (e.g., "4Gi") |
| `limits.cpu`             | Total CPU limits allowed                    |
| `limits.memory`          | Total memory limits allowed                 |

---
##### Tips

- Combine with **LimitRanges** to enforce per-container limits
- Set quotas early in a multi-team environment to avoid conflicts
- Monitor usage with `kubectl describe resourcequota`

---
## 🧾 Commands

```bash
# Example:
kubectl get pods
```
