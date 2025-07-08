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

A Kubernetes namespace is a logical partition within a Kubernetes cluster that allows you to divide resources and manage them independently. It's like a "virtual cluster" within your main cluster. Namespaces help organize and isolate resources (such as pods, services, and deployments) to support:

Multi-tenancy (different teams or projects sharing the same cluster)

Environment separation (e.g., dev, test, staging, prod)

Resource allocation and quota enforcement

Access control via RBAC
###### Purpose of Namespaces
- Logical **separation** of resources
- **Multi-tenancy** in a single cluster
- Simplified **resource management**
- Apply **RBAC** (Role-Based Access Control) per namespace
- Set **resource quotas** per team/project

🔧 Key Concepts of Namespaces

Concept	Description

Isolation	Resources in one namespace can't see or interact with resources in another namespace (unless explicitly allowed).
Default Namespace	If you don’t specify a namespace, Kubernetes uses the default namespace.
System Namespaces	Kubernetes uses special namespaces like kube-system, kube-public, and kube-node-lease.
Scoped Resources	Most Kubernetes resources (pods, services, deployments) are namespace-scoped. Others (nodes, persistent volumes) are cluster-scoped.


```YAML
kind: Namespace
apiVersion: v1
	metadata:
		name: nginx
```

## 🧾 Commands

```bash
# Example:
kubectl 
```
