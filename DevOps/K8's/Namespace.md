# Kubernetes – Namespace

🗓️ **Date:** 2025-07-01  
🕒 **Time:** 23:42  

🏷️ **Tags:** #kubernetes #devops #Namespace  

---

## 📝 Notes

###### Namespace
A **Namespace** in Kubernetes is a way to logically divide cluster
resources. It allows multiple users or teams to share the same cluster
without interfering with each other.
###### Purpose of Namespaces
- Logical **separation** of resources
- **Multi-tenancy** in a single cluster
- Simplified **resource management**
- Apply **RBAC** (Role-Based Access Control) per namespace
- Set **resource quotas** per team/project

```YAML
kind: Namespace
apiVersion: v1
	metadata:
		name: nginx
```

## 🧾 Commands

```bash
# Example:
kubectl get pods
```
