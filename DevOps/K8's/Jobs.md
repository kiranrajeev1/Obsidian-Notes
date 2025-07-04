# Kubernetes – Jobs

🗓️ **Date:** 2025-07-02  
🕒 **Time:** 00:11  

🏷️ **Tags:** #kubernetes #devops #Jobs  

---

## 📝 Notes

##### Job
A **Job** is used to **run a task to completion** in Kubernetes.
It ensures that **a specified number of Pods successfully complete**
their work.

###### Use Cases:
- **Batch processing**
- **Data migrations**
- **One-time tasks**
- **Database backups**
- **Cron-like tasks** (when used with CronJobs)

##### Example

```YAML
apiVersion: batch/v1
kind: Job
metadata:
  name: my-job
spec:
  completions: 1
  parallelism: 1
  template:
    spec:
      containers:
      - name: my-container
        image: busybox
        command: ["echo", "Hello from the Job"]
      restartPolicy: Never
```

##### Key Settings:
- completions: Number of times the Job should successfully complete.
- parallelism: How many Pods can run at the same time.
- restartPolicy: Should be OnFailure or Never.

###### Job Lifecycle:
1.  Job creates one or more Pods.
2.  Each Pod runs to **completion** (success).
3.  Job tracks successful completions.
4.  Once the goal is met, the Job is **done**.


**my code**

```yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: demo-job
  namespace: nginx
spec:
  completions: 1
  parallelism: 1
  template:
    metadata:
      name: demo-job-pod
      labels:
        app: batch-task
    spec:
      containers:
      - name: batch-container
        image: busybox:latest
        command: ["sh", "-c", "echo Hello World! && sleep 5"]
      restartPolicy: Never
```
## 🧾 Commands

```bash
kubectl create -f job.yaml # Create a Job
kubectl get jobs # List Jobs
kubectl describe job <job-name> # Detailed info
kubectl delete job <job-name> # Delete Job
```
