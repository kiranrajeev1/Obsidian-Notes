# Kubernetes – Deployment

🗓️ **Date:** 2025-07-01  
🕒 **Time:** 23:58  

🏷️ **Tags:** #kubernetes #devops #Deployment  

---

## 📝 Notes

** What is a Deployment?**

- A **Deployment** manages **replica sets** and **Pods**.
- Ensures the **desired number of Pods** are running and updated.

 

**🔧 Key Features**

- **Self-healing**: restarts failed Pods.
- **Scaling**: increase or decrease replicas.
- **Rolling updates**: updates Pods without downtime.
- **Rollback**: revert to a previous version if needed.

 

**🧱 Structure**

Basic YAML:

```YAML
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-deployment
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


 

**🎯 Use Cases**

- Auto-replace crashed Pods
- Smooth app updates
- Easy scaling

## 🧾 Commands

**🔥 Common Commands**

```bash
kubectl create -f deployment.yaml # Create deployment
kubectl get deployments # List deployments
kubectl scale deployment my-deploy --replicas=5 # Scale
kubectl rollout status deployment/my-deploy # Check update
kubectl rollout undo deployment/my-deploy # Rollback
```

