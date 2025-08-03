# Kubernetes – Admission Controller

🗓️ **Date:** 03-08-2025  
🕒 **Time:** 23:55  

🏷️ **Tags:** #kubernetes #devops #AdmissionController  

---

## 📝 Notes

##### Kubernetes Admission Controllers

###### Overview

Admission Controllers are **Kubernetes components** that **intercept API requests** (after authentication and authorization) but **before persistence** to **validate, modify, or reject** requests.

They are **plug-ins** that act as **gatekeepers** for your Kubernetes cluster, enforcing policies or injecting defaults automatically.

##### Admission Controller Request Lifecycle

1. **Authentication**    
2. **Authorization**
3. **Admission Controllers**
    - Validating Admission Controllers
    - Mutating Admission Controllers
4. **Persistence in etcd**

Admission Controllers operate **after** the request is authenticated and authorized but **before** the object is stored.

---

##### Types of Admission Controllers

###### Mutating Admission Controllers
- Can **modify** incoming requests (e.g., inject labels, add default values).
- Example: `MutatingAdmissionWebhook`, `DefaultStorageClass`, `LimitRanger`

###### Validating Admission Controllers
- Can **accept or reject** requests based on cluster policies.
- Example: `ValidatingAdmissionWebhook`, `ResourceQuota`, `PodSecurity`

---

##### Built-in Admission Controllers

|Controller|Type|Purpose|
|---|---|---|
|`NamespaceLifecycle`|Validating|Prevents modification/deletion of `kube-system` and enforces namespaces|
|`LimitRanger`|Mutating|Applies default resource limits and ranges|
|`ServiceAccount`|Mutating|Automatically assigns service accounts to pods|
|`NodeRestriction`|Validating|Limits kubelet access to resources it should control|
|`PodSecurity`|Validating|Enforces Pod Security Standards (restricted, baseline, privileged)|
|`ResourceQuota`|Validating|Ensures quotas are not exceeded|
|`DefaultStorageClass`|Mutating|Automatically sets default storage class|
|`TaintNodesByCondition`|Mutating|Adds taints based on node conditions|

---

##### Admission Webhooks

Admission controllers that are **custom and dynamic** are implemented as **webhooks**.
###### Types
- `MutatingAdmissionWebhook`: Can **modify** the object
- `ValidatingAdmissionWebhook`: Can only **accept/reject**

###### Use Cases
- Enforce naming conventions
- Deny use of `latest` image tag
- Inject sidecar containers
- Require specific annotations or labels

###### Registration
Webhooks are registered using these Kubernetes resources:
- `MutatingWebhookConfiguration`
- `ValidatingWebhookConfiguration`

---

##### How to Enable or Disable Admission Controllers

Admission controllers are **enabled via the API server** (`kube-apiserver`) using the `--enable-admission-plugins` flag.

Example:
```bash
kube-apiserver \
  --enable-admission-plugins=NamespaceLifecycle,LimitRanger,PodSecurity \
  --disable-admission-plugins=SomePlugin
```

Note: Many are enabled by default on managed clusters (e.g., GKE, EKS, AKS)

---

##### Order of Execution

1. **Mutating Admission Controllers**    
2. **Validating Admission Controllers**

If both are present, mutation happens before validation.

---

##### Best Practices

- Use admission webhooks for **custom policy enforcement**.    
- Validate webhook availability; misconfigured webhooks can cause **cluster-wide API failures**.
- Use **failurePolicy: Ignore** during early testing.
- Keep webhook services highly available.
- Use **`sideEffects` field** to declare side-effect-free behavior for webhooks.
- Namespace-selective webhooks using `namespaceSelector`.

---

##### Example: Validating Webhook Configuration

```yaml
apiVersion: admissionregistration.k8s.io/v1
kind: ValidatingWebhookConfiguration
metadata:
  name: deny-latest-tag
webhooks:
- name: deny-latest-tag.k8s.io
  rules:
  - apiGroups: [""]
    apiVersions: ["v1"]
    operations: ["CREATE", "UPDATE"]
    resources: ["pods"]
  clientConfig:
    service:
      name: webhook-service
      namespace: default
      path: "/validate"
    caBundle: <PEM_ENCODED_CA>
  admissionReviewVersions: ["v1"]
  sideEffects: None
  failurePolicy: Fail
```

---

##### Tools for Working with Admission Controllers

|Tool|Use Case|
|---|---|
|`kubectl api-resources`|See which resources exist|
|`kubectl apply --dry-run=server`|Trigger admission chain for testing|
|OPA/Gatekeeper|Policy-as-code enforcement with admission|
|Kyverno|Declarative admission controller (policy engine)|
|kube-score|Static analysis tool that simulates validation|

---

##### Summary

- Admission controllers are critical for enforcing security, defaulting, and policy.
- Kubernetes has many built-in controllers; for advanced needs, use admission webhooks.
- They operate between authorization and storage, ensuring compliance and control.
- Familiarity with both built-in and webhook-based controllers is expected in interviews.

---

## 🧾 Commands

```bash
# Example:
kubectl get pods
```
