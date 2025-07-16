# Kubernetes – PersistentVolume

🗓️ **Date:** 16-07-2025  
🕒 **Time:** 10:10  

🏷️ **Tags:** #kubernetes #devops #PersistentVolume  

---

## 📝 Notes

#####  1. What is a Persistent Volume (PV)?

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

###### Fields Breakdown:
- `capacity`: Size of the volume.
- `accessModes`:
    - `ReadWriteOnce` (RWO): One node can read/write.
    - `ReadOnlyMany` (ROX): Many nodes can read.
    - `ReadWriteMany` (RWX): Many nodes can read/write.
- `persistentVolumeReclaimPolicy`: What happens to the volume when it's released. Options:
    - `Retain`: Keeps data after release.
    - `Recycle`: Deprecated (used to wipe data).
    - `Delete`: Deletes the storage backend.
- `storageClassName`: Associates PV with PVCs requesting this class.
---
#####  2. What is a Persistent Volume Claim (PVC)?

A **Persistent Volume Claim (PVC)** is a **request for storage** by a user (typically a developer). It specifies:

- **How much storage is needed**
- **What access mode is required**
- **Which StorageClass to use (if any)**

Kubernetes matches the PVC to a suitable PV (if available), or dynamically provisions one.

###### Example PVC:

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: my-pvc
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 3Gi
  storageClassName: standard
```
###### Key Points:

- The PVC doesn’t need to know the backend (EBS, NFS, etc.).
- It’s an abstraction to **request storage** in a declarative way.
- K8s will bind this claim to a suitable PV.

###### Binding Process: PV PVC

When a PVC is created:

1. Kubernetes searches for a **matching PV** that satisfies the PVC's request (capacity, access mode, and `storageClassName`).
2. If a match is found, the PV is bound to the PVC.
3. If no match is found and dynamic provisioning is enabled (via a StorageClass), K8s will create a new PV for the PVC.
###### Dynamic Provisioning:
If PVC specifies a `storageClassName`, and no PVs match, Kubernetes will provision a volume dynamically using that class.
######  Reclaim Policy
When a PVC is deleted, the PV it was bound to is marked as "Released". What happens next depends on the **reclaim policy**:

| Reclaim Policy | Behavior                                              |
| -------------- | ----------------------------------------------------- |
| `Retain`       | Keeps the data for manual cleanup.                    |
| `Delete`       | Deletes both PV and underlying storage (e.g., EBS).   |
| `Recycle`      | Wipes data and makes PV available again (deprecated). |

###### Access Modes (Detailed)

|Mode|Description|
|---|---|
|`ReadWriteOnce` (RWO)|Mounted as read-write by **a single node**. Most common.|
|`ReadOnlyMany` (ROX)|Mounted as **read-only** by many nodes.|
|`ReadWriteMany` (RWX)|Mounted as **read-write** by many nodes (e.g., NFS, GlusterFS).|

---

## 🧾 Commands

```bash
# Example:
kubectl get pods
```
