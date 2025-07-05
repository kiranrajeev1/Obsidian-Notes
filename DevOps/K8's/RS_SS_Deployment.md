# Kubernetes – RS_SS_Deployment

🗓️ **Date:** 2025-07-02  
🕒 **Time:** 00:00  

🏷️ **Tags:** #kubernetes #devops #RS_SS_Deployment  #ReplicaSet #StatefulSet #Deployment

---

## 📝 Notes

##### ReplicaSet (RS)
**Purpose:**

- Ensures a **specified number of pod replicas** are running at all
  times.

**Key Points:**

- Automatically replaces failed/missing Pods.
- Used **internally by Deployments** (you usually don't create RS
  directly).
- Matches Pods using **labels**.

**Example:**

```YAML
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: my-rs
spec:
  replicas: 3
  selector:
    matchLabels:
      app: myapp
  template:
    metadata:
      labels:
        app: myapp
    spec:
      containers:
      - name: mycontainer
        image: nginx

```

---
##### StatefulSet
###### Purpose:
- Manages **stateful applications** (like databases).

###### Key Features:
- Gives each Pod a **unique, stable identity** (e.g., pod-0, pod-1).
- **Stable storage**: PersistentVolume per Pod.
- **Ordered deployment and scaling**.
- Used when **Pod identity and order** matter.

###### Use Cases:
- Databases (e.g., MySQL, Cassandra)
- Distributed systems (e.g., Kafka, Zookeeper)

###### Example:

```YAML
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: my-db
spec:
  serviceName: "my-service"
  replicas: 3
  selector:
    matchLabels:
      app: mydb
  template:
    metadata:
      labels:
        app: mydb
    spec:
      containers:
      - name: db
        image: mysql

```

 ---
#####  Deployment

###### Purpose:
- Manages **stateless applications**.
- Handles **rolling updates**, **rollbacks**, and **scaling**.

###### Key Features:
- Manages ReplicaSets.
- Best for apps **without persistent state**.
- Supports **rolling updates** to avoid downtime.

###### Example:

```YAML
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-deploy
spec:
  replicas: 3
  selector:
    matchLabels:
      app: webapp
  template:
    metadata:
      labels:
        app: webapp
    spec:
      containers:
      - name: web
        image: nginx

```


## 🧾 Commands

```bash
# Example:
kubectl get pods
```
