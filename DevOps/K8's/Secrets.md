# Kubernetes – Secrets

🗓️ **Date:** 22-07-2025  
🕒 **Time:** 21:35  

🏷️ **Tags:** #kubernetes #devops #Secrets  

---

## 📝 Notes

In **Kubernetes (k8s)**, a **Secret** is an object used to store sensitive data such as passwords, OAuth tokens, SSH keys, and TLS certificates. The main purpose of Secrets is to keep confidential data separate from application code and configuration.

---

##### What Are Kubernetes Secrets?

A **Secret** is a Kubernetes object of type `Secret`, which stores data in base64-encoded format. It allows you to:

- Avoid hardcoding sensitive information in **Pod specs** or **ConfigMaps**.
- Grant access to the secret only to components (e.g., Pods) that need it.
- Keep secrets encrypted at rest (when configured).
- Dynamically update secrets without restarting the pods (depending on how they are mounted).

---

##### Types of Secrets

Kubernetes supports several types of secrets:

|Type|Description|
|---|---|
|`Opaque`|Default type, arbitrary key-value pairs (user-defined).|
|`kubernetes.io/basic-auth`|Username and password pair.|
|`kubernetes.io/ssh-auth`|SSH private key.|
|`kubernetes.io/tls`|TLS certificate and private key.|
|`kubernetes.io/dockerconfigjson`|Docker registry credentials.|

---

##### How to Create a Secret

###### 1. **Using a YAML manifest**

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: my-secret
type: Opaque
data:
  username: dXNlcm5hbWU=     # "username" base64 encoded
  password: cGFzc3dvcmQ=     # "password" base64 encoded
```

Apply it with:

```bash
kubectl apply -f my-secret.yaml
```

###### 2. **Using `kubectl`**

```bash
kubectl create secret generic my-secret \
  --from-literal=username=myuser \
  --from-literal=password=mypass
```

---

##### Consuming a Secret

You can use a secret in a pod in three main ways:

###### 1. **As environment variables**

```yaml
env:
- name: DB_USERNAME
  valueFrom:
    secretKeyRef:
      name: my-secret
      key: username
```

###### 2. **As volumes (files in the container)**

```yaml
volumes:
- name: secret-volume
  secret:
    secretName: my-secret
```

```yaml
volumeMounts:
- name: secret-volume
  mountPath: "/etc/secret"
  readOnly: true
```

###### 3. **As imagePullSecrets**

Used to authenticate to private container registries.

---

##### Security Considerations

- Secrets are **base64-encoded**, not encrypted by default.
- Enable **encryption at rest** using KMS providers.
- Access control is crucial—**use RBAC** to limit who can access secrets.
- Avoid logging secrets or exposing them via `kubectl describe` carelessly.

---

##### Viewing a Secret

To decode a secret:

```bash
kubectl get secret my-secret -o yaml
```

To decode values:

```bash
echo 'dXNlcm5hbWU=' | base64 --decode
```

##### ConfigMaps vs. Secrets: Key Differences

| Feature            | ConfigMap                                        | Secret                                                |
| :----------------- | :----------------------------------------------- | :---------------------------------------------------- |
| **Purpose**        | Non-sensitive configuration data.                | Sensitive data (passwords, keys, certs).              |
| **Data Encoding**  | Plain text.                                      | Base64 encoded (obfuscation, NOT encryption).         |
| **Encryption**     | No built-in encryption.                          | Requires `etcd` encryption at rest for true security. |
| **Access Control** | Standard RBAC.                                   | Critical to apply strict RBAC for restricted access.  |
| **Visibility**     | Easily readable in plain text via `kubectl get`. | Base64-encoded via `kubectl get`, requires decoding.  |
|**Use Cases**|Environment variables, URLs, feature flags, config files.|Passwords, API keys, TLS certs, SSH keys, registry credentials.|


## 🧾 Commands

##### Secret Creation Commands

###### Create a Secret from literals

```bash
kubectl create secret generic my-secret \
  --from-literal=username=myuser \
  --from-literal=password=mypass
```

###### Create from a file

```bash
kubectl create secret generic my-secret \
  --from-file=./username.txt \
  --from-file=./password.txt
```

###### Create from a directory (each file becomes a key)

```bash
kubectl create secret generic my-secret \
  --from-file=./secrets/
```

###### Create a Docker registry secret

```bash
kubectl create secret docker-registry my-docker-secret \
  --docker-username=myuser \
  --docker-password=mypass \
  --docker-email=myemail@example.com \
  --docker-server=https://index.docker.io/v1/
```

---

##### View and Manage Secrets

###### List all secrets

```bash
kubectl get secrets
```

###### Get details of a specific secret (base64-encoded)

```bash
kubectl get secret my-secret -o yaml
```

###### Decode a secret value

```bash
kubectl get secret my-secret -o jsonpath="{.data.username}" | base64 --decode
```

###### Describe a secret (metadata only, values not shown)

```bash
kubectl describe secret my-secret
```

##### Edit / Update / Delete Secrets

###### Edit a secret (opens in default editor)

```bash
kubectl edit secret my-secret
```

###### Delete a secret

```bash
kubectl delete secret my-secret
```

##### Use Secrets in Pods

###### Use as environment variables in a pod

```yaml
env:
- name: DB_USER
  valueFrom:
    secretKeyRef:
      name: my-secret
      key: username
```

###### Use as a volume in a pod

```yaml
volumes:
- name: secret-vol
  secret:
    secretName: my-secret

volumeMounts:
- name: secret-vol
  mountPath: "/etc/secret"
```

###### Use as imagePullSecret for private registries

```yaml
imagePullSecrets:
- name: my-docker-secret
```

##### Import from YAML

###### Apply a Secret from a manifest file

```bash
kubectl apply -f my-secret.yaml
```

##### (Optional) Encrypt Secrets at Rest

> Secrets can be encrypted at rest by configuring the Kubernetes API server. This cannot be done via `kubectl`. Let me know if you want those steps.

