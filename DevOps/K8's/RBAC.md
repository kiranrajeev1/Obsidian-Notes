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

##### 1. Creating RBAC Resources

###### Create a Role

```bash
kubectl create role pod-reader \
  --verb=get,list,watch \
  --resource=pods \
  --namespace=dev
```

###### Create a ClusterRole

```bash
kubectl create clusterrole node-reader \
  --verb=get,list,watch \
  --resource=nodes
```

###### Create a RoleBinding

```bash
kubectl create rolebinding read-pods \
  --role=pod-reader \
  --user=alice \
  --namespace=dev
```

###### Create a ClusterRoleBinding

```bash
kubectl create clusterrolebinding admin-binding \
  --clusterrole=cluster-admin \
  --user=admin@example.com
```

###### Create RoleBinding for a ServiceAccount

```bash
kubectl create rolebinding sa-pod-access \
  --role=pod-reader \
  --serviceaccount=dev:my-sa \
  --namespace=dev
```

---

##### 2. Viewing and Inspecting RBAC Resources

###### List Roles and RoleBindings

```bash
kubectl get roles -n dev
kubectl get rolebindings -n dev
```

###### List ClusterRoles and ClusterRoleBindings

```bash
kubectl get clusterroles
kubectl get clusterrolebindings
```

###### Describe RBAC Resources

```bash
kubectl describe role pod-reader -n dev
kubectl describe rolebinding read-pods -n dev
kubectl describe clusterrole node-reader
kubectl describe clusterrolebinding admin-binding
```

###### View All API Resources and Groups

```bash
kubectl api-resources
kubectl api-versions
```

###### Explain RBAC Resource Structure

```bash
kubectl explain role
kubectl explain role.rules
kubectl explain clusterrole.rules.verbs
```

---

##### 3. Validating Access

###### Check What a User/SA Can Do

```bash
kubectl auth can-i list pods --as=alice --namespace=dev
kubectl auth can-i create deployments --as=system:serviceaccount:dev:my-sa
```

###### List Everything a User/SA Can Do

```bash
kubectl auth can-i --list --as=system:serviceaccount:dev:my-sa
```

###### Check Using Current User Context

```bash
kubectl auth can-i delete services
```

---

##### 4. Troubleshooting RBAC Issues

###### Who Can Access a Resource (requires plugin)

```bash
kubectl who-can get pods
```

###### Inspect Bound Roles

```bash
kubectl get rolebinding -n dev -o wide
kubectl get clusterrolebinding -o yaml | grep -B5 -A10 "subject"
```

###### Debug Pod API Access

```bash
kubectl exec -it my-pod -n dev -- curl -sSk https://kubernetes.default.svc/api --header "Authorization: Bearer $(cat /var/run/secrets/kubernetes.io/serviceaccount/token)"
```

---

##### 5. Deleting RBAC Resources

###### Delete Role and RoleBinding

```bash
kubectl delete role pod-reader -n dev
kubectl delete rolebinding read-pods -n dev
```

###### Delete ClusterRole and ClusterRoleBinding

```bash
kubectl delete clusterrole node-reader
kubectl delete clusterrolebinding admin-binding
```

---

##### 6. Advanced & Useful Shortcuts

###### Create from YAML

```bash
kubectl apply -f rbac.yaml
kubectl delete -f rbac.yaml
```

###### Dry Run RBAC Resources

```bash
kubectl create role test-role \
  --verb=get,list \
  --resource=pods \
  --dry-run=client -o yaml
```

---

##### Bonus: Helpful Plugins and Tools

###### rakkess – Access Matrix (plugin)

```bash
kubectl rakkess --as=system:serviceaccount:dev:my-sa
```

###### rbac-lookup – Reverse RBAC Lookup

```bash
rbac-lookup alice
```

---

