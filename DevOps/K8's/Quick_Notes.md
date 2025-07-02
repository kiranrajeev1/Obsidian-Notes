# Kubernetes – Quick_Notes

🗓️ **Date:** 2025-07-01  
🕒 **Time:** 23:41  

🏷️ **Tags:** #kubernetes #devops #Quick_Notes  

---

## 📝 Notes

**Kubernetes Quick Notes**

- **Pod** -- An abstraction over a container. The **smallest deployable
  unit** in Kubernetes, which can hold one or more containers sharing
  storage, network, and a specification.

- **Node** -- A **server** (virtual or physical) in the Kubernetes
  cluster that runs workloads.

  - Two types of nodes:

    - **Master Node (Control Plane Node)** -- Manages the cluster and
      makes global decisions.

    - **Worker Node** -- Runs application Pods.

- **Cluster** -- A group of nodes (master and worker) that run
  containerized applications managed by Kubernetes.

- **Kubelet** -- A Kubernetes **agent** that runs on each node and
  ensures containers in Pods are running as specified; it communicates
  with the control plane and manages container lifecycle.

- **API Server** -- The **entry point** to the Kubernetes cluster that
  processes RESTful API requests and handles communication between
  users, components, and the control plane.

- **Controller Manager** -- A **control plane component** that runs
  various controllers (like Node, Replication, Endpoint controllers) to
  maintain the cluster's desired state.

- **Scheduler** -- A control plane component that **selects the optimal
  node** for a Pod based on available resources, constraints, and
  policies.

- **etcd** -- A **highly available key-value store** used as Kubernetes'
  **backing store** for all cluster state and configuration data.

- **Virtual Network** -- A **software-defined network (SDN)** that
  allows Pods on different nodes to **communicate via IP**, and also
  enables communication with external services.

- **Service** - A Service in Kubernetes is a way to give a stable name
  and IP address to a group of Pods, so they can be easily accessed even
  if the Pods change or restart

## 🧾 Commands

```bash
# Example:
kubectl get pods
```
