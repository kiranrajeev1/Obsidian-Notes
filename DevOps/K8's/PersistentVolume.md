# Kubernetes – PersistentVolume

🗓️ **Date:** 16-07-2025  
🕒 **Time:** 10:10  

🏷️ **Tags:** #kubernetes #devops #PersistentVolume  #PV #PVC

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

Sure! Here's the complete list of **Kubernetes Persistent Volume (PV)** and **Persistent Volume Claim (PVC)** commands in **Markdown format**, with **unnecessary line breaks removed** and using `#####` and `######` headings.

---

##### Persistent Volume (PV) & Persistent Volume Claim (PVC) Commands

##### Create Persistent Volumes and Claims

###### Create a Persistent Volume (PV) from a YAML file

```bash
kubectl apply -f pv.yaml
```

###### Create a Persistent Volume Claim (PVC) from a YAML file

```bash
kubectl apply -f pvc.yaml
```

##### List and Get PVs and PVCs

###### List all Persistent Volumes

```bash
kubectl get pv
```

###### List all Persistent Volume Claims in the current namespace

```bash
kubectl get pvc
```

###### Get detailed information about a specific PV

```bash
kubectl describe pv <pv-name>
```

###### Get detailed information about a specific PVC

```bash
kubectl describe pvc <pvc-name>
```

##### Delete PVs and PVCs

###### Delete a Persistent Volume

```bash
kubectl delete pv <pv-name>
```

###### Delete a Persistent Volume Claim

```bash
kubectl delete pvc <pvc-name>
```

##### Edit and Patch PVs and PVCs

###### Edit a Persistent Volume

```bash
kubectl edit pv <pv-name>
```

###### Edit a Persistent Volume Claim

```bash
kubectl edit pvc <pvc-name>
```

###### Patch a PVC (e.g., update storage request)

```bash
kubectl patch pvc <pvc-name> -p '{"spec":{"resources":{"requests":{"storage":"2Gi"}}}}'
```

##### Monitor PV/PVC Usage and Binding

###### Watch PVC status in real time

```bash
kubectl get pvc -w
```

###### See which PV is bound to which PVC

```bash
kubectl get pv
```

###### Check events related to PVC binding issues

```bash
kubectl describe pvc <pvc-name>
```

##### Use PVs and PVCs in Pods

###### Example pod using a PVC

```yaml
volumes:
- name: my-storage
  persistentVolumeClaim:
    claimName: my-pvc

volumeMounts:
- mountPath: "/data"
  name: my-storage
```

##### StorageClass Related (if dynamic provisioning is used)

###### List all StorageClasses

```bash
kubectl get storageclass
```

###### Describe a specific StorageClass

```bash
kubectl describe storageclass <name>
```

---

Let me know if you’d like YAML templates for static or dynamic provisioning examples.