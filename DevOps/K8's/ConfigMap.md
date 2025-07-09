# Kubernetes – ConfigMap

🗓️ **Date:** 09-07-2025  
🕒 **Time:** 16:09  

🏷️ **Tags:** #kubernetes #devops #ConfigMap  

---

## 📝 Notes

### 🧾 What is Configuration?

**Configuration** refers to the **settings, parameters, or options** that control how software, systems, or services behave.

---

### ✅ In Simple Terms:

> Configuration is **how you tell software what to do** without changing its code.
##### 🗂️ Kubernetes ConfigMap

###### 📌 What is a ConfigMap?

A **ConfigMap** is a Kubernetes object used to **store non-confidential configuration data** in **key-value** pairs. It helps decouple environment-specific configurations from application code.

###### 📄 Use Cases

- Store configuration files (e.g., `app.properties`, `settings.json`)
- Store environment variables for pods
- Centralized management of app configurations

###### 🧱 Creating a ConfigMap

###### 1. From Literal Values

```bash
kubectl create configmap my-config --from-literal=key1=value1 --from-literal=key2=value2
```

###### 2. From a File

```bash
kubectl create configmap my-config --from-file=config.txt
```

###### 3. From a Directory (multiple files)

```bash
kubectl create configmap my-config --from-file=/path/to/config-dir/
```

###### 4. From a YAML File

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: my-config
data:
  key1: value1
  key2: |
    multi-line
    value
```

**Apply with:**

```bash
kubectl apply -f configmap.yaml
```

---

##### 📥 Accessing ConfigMap in Pods

###### 1. As Environment Variables

```yaml
envFrom:
  - configMapRef:
      name: my-config
```

###### 2. As Individual Env Vars

```yaml
env:
  - name: MY_KEY
    valueFrom:
      configMapKeyRef:
        name: my-config
        key: key1
```

###### 3. As Volume Mount

```yaml
volumes:
  - name: config-volume
    configMap:
      name: my-config
containers:
  - name: my-container
    volumeMounts:
      - name: config-volume
        mountPath: /etc/config
```

##### 🧪 Viewing ConfigMap

```bash
kubectl get configmap my-config -o yaml
```

> ⚠️ Note: Pods do not automatically reload ConfigMap changes. You must restart the pod or use tools like [Reloader](https://github.com/stakater/Reloader) or [Kustomize patches].

---

##### 🆚 ConfigMap vs Secret

| Feature    | ConfigMap    | Secret           |
| ---------- | ------------ | ---------------- |
| Use for    | Plain config | Sensitive data   |
| Encoded?   | No           | Base64 encoded   |
| Encrypted? | No           | Yes (optionally) |

## 🧾 Commands

```bash
# Example:
kubectl get pods
```
