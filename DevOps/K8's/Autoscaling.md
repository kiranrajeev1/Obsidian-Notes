# Kubernetes – Autoscaling

🗓️ **Date:** 23-07-2025  
🕒 **Time:** 23:13  

🏷️ **Tags:** #kubernetes #devops #Autoscaling  

---

## 📝 Notes

##### Autoscaling
Kubernetes (K8s) **autoscaling** is the mechanism by which Kubernetes automatically adjusts the number of **pods** or **nodes** in your cluster based on demand. This helps ensure your application is responsive under load and cost-effective during low demand.
##### HPA (Horizontal Pod Autoscaler)
Kubernetes **HPA (Horizontal Pod Autoscaler)** is a built-in controller that automatically scales the number of pods in a **Deployment**, **ReplicaSet**, or **StatefulSet** based on observed resource usage like CPU, memory, or custom metrics.

###### **How HPA Works**
1. **Metrics Collection**: HPA uses the **Metrics Server** to collect resource usage data.
2. **Target Utilization**: You define a target, such as "average CPU usage should be 60%".
3. **Scaling Logic**: If actual usage exceeds the target, HPA adds pods; if it’s below, it removes pods.
4. **Periodicity**: HPA checks metrics every **15 seconds by default** and may scale every **30 seconds**.
###### Example: CPU-based HPA
```yaml
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: my-app-hpa
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: my-app
  minReplicas: 2
  maxReplicas: 10
  metrics:
  - type: Resource
    resource:
      name: cpu
      target:
        type: Utilization
        averageUtilization: 60
```

This tells Kubernetes:
- Keep pod count between **2 and 10**.
- Target **60% average CPU usage** across all pods.

###### Key Concepts

|Concept|Description|
|---|---|
|`minReplicas`|Minimum number of pods to maintain|
|`maxReplicas`|Maximum number of pods to scale up to|
|`metrics`|Defines what metrics to monitor (CPU, memory, custom)|
|`scaleTargetRef`|Points to the workload (e.g., Deployment) to scale|

###### Custom Metrics Support
HPA can also use:
- **Memory usage**
- **Custom application metrics** via Prometheus + Adapter
- **External metrics** like queue length, request rate, etc.
This requires setting up custom metric adapters.

###### Requirements
- **Metrics Server** must be installed and running.
- Target pods should expose CPU/memory requests for metrics collection to work effectively.

###### When to Use HPA?
Use HPA when:
- Your app load varies dynamically.
- You want automatic scaling without human intervention.
- You have predictable resource metrics.

Would you like an example using **memory** or **custom metrics** as well?

---
## 🧾 Commands

```bash
# Example:
kubectl get pods
```
