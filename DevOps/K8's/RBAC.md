# Kubernetes – RBAC

🗓️ **Date:** 03-08-2025  
🕒 **Time:** 23:47  

🏷️ **Tags:** #kubernetes #devops #RBAC  

---

## 📝 Notes

##### Kubernetes RBAC (Role-Based Access Control)
###### Overview
RBAC in Kubernetes is a method of regulating access to resources based on the roles of users or service accounts. It enables fine-grained permission control using Role, ClusterRole, RoleBinding, and ClusterRoleBinding.

###### RBAC Resources

| Resource             | Scope     | Description                                              |
| -------------------- | --------- | -------------------------------------------------------- |
| `Role`               | Namespace | Grants permissions within a specific namespace           |
| `ClusterRole`        | Cluster   | Grants permissions cluster-wide or across all namespaces |
| `RoleBinding`        | Namespace | Grants the permissions defined in a Role to subjects     |
| `ClusterRoleBinding` | Cluster   | Grants ClusterRole permissions across the cluster        |

##### 1. Roles and ClusterRoles

###### Role
- Namespace-scoped
- Grants specific actions on resources within the namespace

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  namespace: dev
  name: pod-reader
rules:
- apiGroups: [""]
  resources: ["pods"]
  verbs: ["get", "list", "watch"]
```

###### ClusterRole

- Cluster-scoped or namespace-agnostic
- Can be bound in any namespace using RoleBinding or cluster-wide using ClusterRoleBinding

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  name: node-reader
rules:
- apiGroups: [""]
  resources: ["nodes"]
  verbs: ["get", "list", "watch"]
```

---

##### 2. RoleBinding and ClusterRoleBinding

###### RoleBinding
- Binds Role or ClusterRole to a user, group, or service account within a specific namespace

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: read-pods
  namespace: dev
subjects:
- kind: User
  name: alice
  apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: Role
  name: pod-reader
  apiGroup: rbac.authorization.k8s.io
```

###### ClusterRoleBinding

- Binds a ClusterRole to subjects across the entire cluster

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: admin-access
subjects:
- kind: Group
  name: system:admins
  apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: ClusterRole
  name: cluster-admin
  apiGroup: rbac.authorization.k8s.io
```

---

##### 3. Subjects

|Kind|Description|
|---|---|
|`User`|Represents a real end user|
|`Group`|A collection of users|
|`ServiceAccount`|Used by pods to interact with the API server|
|`SystemUser`|Used by Kubernetes components (e.g. controller)|
|`SystemGroup`|Group of system users|

---

##### 4. Verbs and Resources

###### Common Verbs

- `get`, `list`, `watch`: Read operations
- `create`, `update`, `patch`, `delete`: Write operations
- `use`: Common with PodSecurityPolicies and Secrets
- `bind`, `escalate`: Advanced RBAC operations

###### Common Resources

| API Group                   | Resources                        |
| --------------------------- | -------------------------------- |
| Core (`""`)                 | `pods`, `services`, `configmaps` |
| `apps`                      | `deployments`, `statefulsets`    |
| `rbac.authorization.k8s.io` | `roles`, `rolebindings`          |
| `batch`                     | `jobs`, `cronjobs`               |

---

##### 5. Checking Access

Check if a user can perform an action:

```bash
kubectl auth can-i get pods --as=alice --namespace=dev
```

List all access for a user/service account:

```bash
kubectl auth can-i --list --as=system:serviceaccount:dev:my-sa
```

---

##### 6. Best Practices

###### Principle of Least Privilege
Grant the minimum set of permissions required.

###### Use Namespaces
Isolate resources and access within namespaces using Role and RoleBinding.

###### Use Groups
Grant roles to groups (e.g., `devs`, `ops`) for scalable access control.

###### Reuse ClusterRoles
Define common permissions as ClusterRoles and bind them as needed.

###### Audit RBAC
Use external tools to detect over-permissioned accounts.

###### Version Control
Store RBAC YAML files in Git repositories for versioning and reviews.

---

##### 7. Common Pitfalls

| Problem                                 | Solution                                          |
| --------------------------------------- | ------------------------------------------------- |
| Incorrect namespace in RoleBinding      | Match RoleBinding and Role in the same namespace  |
| Typos in verbs or resources             | Use `kubectl api-resources` and `kubectl explain` |
| Using ClusterRoleBinding unnecessarily  | Use RoleBinding for scoped permissions            |
| Escalated access via shared ClusterRole | Review all bindings to sensitive ClusterRoles     |

---

##### 8. Tools

| Tool                 | Purpose                            |
| -------------------- | ---------------------------------- |
| `kubectl auth can-i` | Test permissions for a subject     |
| `kubeaudit`          | Audits security and RBAC settings  |
| `rbac-lookup`        | Find who has a specific permission |
| `rakkess`            | Displays user access matrix        |

---
## 🧾 Commands

```bash
# Example:
kubectl get pods
```
