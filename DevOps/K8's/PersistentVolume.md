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
###### StorageClass

A **StorageClass** defines the type of storage and how to provision it. Used with PVCs for dynamic provisioning.

###### Example:
```yaml
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: standard
provisioner: kubernetes.io/aws-ebs
parameters:
  type: gp2
```

PVC with this `storageClassName: standard` will trigger this provisioning logic.

---

###### Lifecycle Summary

1. **Admin** defines PVs or StorageClasses.
2. **User** creates PVCs.
3. K8s matches PVCs with PVs or dynamically provisions.
4. Pods reference PVCs to mount storage volumes.

###### Pod Using PVC Example
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: pod-using-pvc
spec:
  containers:
    - name: app
      image: nginx
      volumeMounts:
        - mountPath: "/usr/share/nginx/html"
          name: my-storage
  volumes:
    - name: my-storage
      persistentVolumeClaim:
        claimName: my-pvc
```

---

###### Real-World Use Cases

- Hosting databases (MySQL, PostgreSQL) with persistent storage.
- Sharing files between pods (if RWX is supported).
- Retaining logs or user data across pod restarts.

---

## 🧾 Commands

# 📘 Kubernetes PV & PVC Notes

---

## 📂 1. Create a Persistent Volume (Manual Provisioning)

# pv.yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: manual-pv
spec:
  capacity:
    storage: 5Gi
  accessModes:
    - ReadWriteOnce
  persistentVolumeReclaimPolicy: Retain
  storageClassName: manual
  hostPath:
    path: "/mnt/data"

# Apply PV
kubectl apply -f pv.yaml

---

## 📦 2. Create a Persistent Volume Claim (Manual Binding)

# pvc.yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: manual-pvc
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 5Gi
  storageClassName: manual

# Apply PVC
kubectl apply -f pvc.yaml

---

## 📌 3. Check PV and PVC Status

kubectl get pv
kubectl get pvc
kubectl describe pv manual-pv
kubectl describe pvc manual-pvc

---

## 📁 4. Create a Pod That Uses the PVC

# pod-using-pvc.yaml
apiVersion: v1
kind: Pod
metadata:
  name: pvc-demo-pod
spec:
  containers:
    - name: app
      image: nginx
      volumeMounts:
        - mountPath: /usr/share/nginx/html
          name: storage
  volumes:
    - name: storage
      persistentVolumeClaim:
        claimName: manual-pvc

# Apply Pod
kubectl apply -f pod-using-pvc.yaml

---

## ⚙️ 5. Create a StorageClass (For Dynamic Provisioning)

# 🔧 Change 'provisioner' according to your environment
# storage-class.yaml
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: standard
provisioner: kubernetes.io/aws-ebs
parameters:
  type: gp2
reclaimPolicy: Delete

# Apply StorageClass
kubectl apply -f storage-class.yaml

---

## 🧾 6. Create a PVC with Dynamic Provisioning

# dynamic-pvc.yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: dynamic-pvc
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 5Gi
  storageClassName: standard

# Apply Dynamic PVC
kubectl apply -f dynamic-pvc.yaml

---

## 🚀 7. Deploy Pod Using Dynamically Provisioned PVC

# pod-dynamic.yaml
apiVersion: v1
kind: Pod
metadata:
  name: dynamic-pod
spec:
  containers:
    - name: app
      image: nginx
      volumeMounts:
        - mountPath: /usr/share/nginx/html
          name: storage
  volumes:
    - name: storage
      persistentVolumeClaim:
        claimName: dynamic-pvc

# Apply Pod
kubectl apply -f pod-dynamic.yaml

---

## 🧹 8. Delete Resources

# Manual
kubectl delete -f pod-using-pvc.yaml
kubectl delete -f pvc.yaml
kubectl delete -f pv.yaml

# Dynamic
kubectl delete -f pod-dynamic.yaml
kubectl delete -f dynamic-pvc.yaml
kubectl delete -f storage-class.yaml

---

## 🔍 9. Useful Watch Commands

watch kubectl get pv,pvc
watch kubectl describe pvc manual-pvc
```
