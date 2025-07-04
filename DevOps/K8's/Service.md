# Kubernetes – Service

🗓️ **Date:** 2025-07-04  
🕒 **Time:** 10:08  

🏷️ **Tags:** #kubernetes #devops #Service  

---

## 📝 Notes

In **Kubernetes**, a **Service** is an abstraction that defines a logical set of **Pods** and a policy by which to access them. Since Pods in Kubernetes are **ephemeral** (they can be created or destroyed anytime), their IPs can change. A **Service** provides a stable IP and DNS name for a set of Pods and helps them communicate internally or externally.
or


##### Why Kubernetes Service is Needed?

- Pods have dynamic IPs → hard to track
- Services offer stable access to these Pods
- Allow **load balancing**, **discovery**, and **communication**

##### How It Works

- A **Service** selects Pods using **labels** (like `app: frontend`)
- It gets a stable **ClusterIP** (virtual IP)
- Traffic sent to the Service is load-balanced across matching Pods

## 🧾 Commands

```bash
# Example:
kubectl get pods
```
