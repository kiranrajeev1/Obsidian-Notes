# Kubernetes – Ingress

🗓️ **Date:** 04-08-2025  
🕒 **Time:** 00:00  

🏷️ **Tags:** #kubernetes #devops #Ingress  

---

## 📝 Notes

##### Ingress Workflow Diagram

```mermaid
flowchart TD
  Client[Client (e.g., Browser)]
  IngressIP[External IP (DNS -> LoadBalancer or NodePort)]
  IngressController[Ingress Controller (e.g., NGINX)]
  Ingress[Ingress Resource (Routing Rules)]
  Service[Kubernetes Service]
  Pod[Backend Pod]

  Client -->|HTTP/HTTPS Request| IngressIP
  IngressIP --> IngressController
  IngressController --> Ingress
  Ingress -->|Route Match (Host/Path)| Service
  Service --> Pod
```

---

## 🧾 Commands

```bash
# Example:
kubectl get pods
```
