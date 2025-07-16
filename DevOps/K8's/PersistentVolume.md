# Kubernetes – PersistentVolume

🗓️ **Date:** 16-07-2025  
🕒 **Time:** 10:10  

🏷️ **Tags:** #kubernetes #devops #PersistentVolume  

---

## 📝 Notes

##### 🧱 1. What is a Persistent Volume (PV)?

A **Persistent Volume (PV)** is a piece of storage in the cluster that has been **provisioned by an administrator** or **dynamically provisioned using Storage Classes**. It is a **resource in the cluster**, just like a node or pod.

###### Key Points:

- PV is a **Kubernetes abstraction** for physical storage.
- It represents a **real storage resource** (e.g., AWS EBS, GCE Persistent Disk, NFS, iSCSI, Ceph, etc.).
- Created **either manually** by a cluster admin or **automatically** via a **StorageClass**.

###### Example PV:
```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: my-pv
spec:
  capacity:
    storage: 5Gi
  accessModes:
    - ReadWriteOnce
  persistentVolumeReclaimPolicy: Retain
  storageClassName: standard
  hostPath:
    path: "/mnt/data"
```

---

## 🧾 Commands

```bash
# Example:
kubectl get pods
```
