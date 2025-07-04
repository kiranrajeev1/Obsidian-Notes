# Kubernetes – Pods

🗓️ **Date:** 2025-07-01  
🕒 **Time:** 23:45  

🏷️ **Tags:** #kubernetes #devops #Pods  

---

## 📝 Notes

###### Pods
- **Pod** is the **smallest and simplest deployable unit** in
  Kubernetes.
- It represents a **single instance of a running process** in your
  cluster.
- A Pod **encapsulates one or more containers**, storage, a unique
  network IP, and options for how the container(s) should run.

###### Key Characteristics
- **Single IP address per Pod** (shared by all containers in the Pod).
- **Tightly coupled containers** in a Pod share:
  - Network namespace
  - Volumes (shared storage)
- **Designed to be ephemeral** -- not self-healing.

######  Use Cases
- **Single-container Pod**: most common scenario.
- **Multi-container Pod**: containers that are tightly coupled (e.g.,
  sidecar pattern -- log shipper, proxy, etc.).

######  Pod Lifecycle Phases
1.  **Pending** -- Pod is accepted but not yet scheduled.
2.  **Running** -- Pod is bound to a node and containers are running.
3.  **Succeeded** -- All containers have terminated successfully.
4.  **Failed** -- At least one container failed.
5.  **Unknown** -- State can't be determined.


###### Pod Management
Pods are usually **not created directly**; they are managed by
higher-level objects:
- **Deployments**
- **ReplicaSets**
- **StatefulSets**
- **DaemonSets**

###### Example

```YAML
apiVersion: v1
kind: Pod
metadata:
  name: mypod
spec:
  containers:
  - name: mycontainer
    image: nginx
    ports:
    - containerPort: 80
```

###### Health Checks
- **Liveness probe** -- Checks if the container is still running.
- **Readiness probe** -- Checks if the container is ready to serve
  traffic.
- **Startup probe** -- Checks if the container has started properly.

###### Volume Mounts
- Pods support different **volume types** like emptyDir, hostPath,
  configMap, secret, etc.
- Volumes are shared between containers in the same Pod.

###### Limitations
- Not designed for **long-term persistence**.
- Pods may be **rescheduled** to a different node if the node fails.
## 🧾 Commands

```bash
kubectl get pods # List all pods
kubectl describe pod \<pod-name\> # Detailed info about a pod
kubectl logs \<pod-name\> # View logs from a pod
kubectl exec -it \<pod-name\> \-- bash # Access pod shell
kubectl delete pod \<pod-name\> # Delete a pod
```

