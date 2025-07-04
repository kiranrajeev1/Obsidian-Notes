# Kubernetes – Service

🗓️ **Date:** 2025-07-04  
🕒 **Time:** 10:08  

🏷️ **Tags:** #kubernetes #devops #Service  

---

## 📝 Notes

##### Service

In **Kubernetes**, a **Service** is an abstraction that defines a logical set of **Pods** and a policy by which to access them. Since Pods in Kubernetes are **ephemeral** (they can be created or destroyed anytime), their IPs can change. A **Service** provides a stable IP and DNS name for a set of Pods and helps them communicate internally or externally.

or

The definition of Kubernetes services]refers to a" Service which is a method for exposing a network application that is running as one or more Pods in your cluster." Service is a static IP address or a permanent IP address that can be attached to the Pod. We need Service because the life cycles of Service and the Pod are not connected, so even if the Pod dies, the Service and its IP address will stay so we don't have to change that endpoint every time the Pod dies.

##### Why Kubernetes Service is Needed?

- Pods have dynamic IPs → hard to track
- Services offer stable access to these Pods
- Allow **load balancing**, **discovery**, and **communication**

##### How It Works

- A **Service** selects Pods using **labels** (like `app: frontend`)
- It gets a stable **ClusterIP** (virtual IP)
- Traffic sent to the Service is load-balanced across matching Pods

##### Example

```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-service
spec:
  selector:
    app: my-app
  ports:
    - protocol: TCP
      port: 80        # Port that the service exposes
      targetPort: 8080 # Port that the pod is listening on
  type: ClusterIP
```

This Service forwards traffic from `my-service:80` to all Pods with `app: my-app` on port `8080`.

---
ClusterIP
## 🧾 Commands

```bash
# Example:
kubectl get pods
```
