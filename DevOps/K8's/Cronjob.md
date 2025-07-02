# Kubernetes – Cronjob

🗓️ **Date:** 2025-07-02  
🕒 **Time:** 00:16  

🏷️ **Tags:** #kubernetes #devops #Cronjob  

---

## 📝 Notes

##### What is a CronJob?

A **CronJob** is used to **run Jobs on a schedule**, just like a Linux
cron.
 

##### Use Cases:

- **Scheduled backups**
- **Daily reports**
- **Database cleanup**
- **Regular batch jobs**

 
##### Basic YAML Example:

```yaml
apiVersion: batch/v1
kind: CronJob
metadata:
  name: my-cronjob
spec:
  schedule: "*/5 * * * *"  # Every 5 minutes
  jobTemplate:
    spec:
      template:
        spec:
          containers:
          - name: my-container
            image: busybox
            command: ["echo", "Hello from CronJob"]
          restartPolicy: OnFailure
```

 

##### Key Fields:

- schedule: **Cron expression** (e.g., \"0 2 \* \* \*\" = every day at 2
  AM)
- jobTemplate: Template for the Job to run.
- successfulJobsHistoryLimit: How many successful Job records to keep.
- failedJobsHistoryLimit: How many failed Job records to keep.
- concurrencyPolicy:
- Allow (default): Jobs can run in parallel.
- Forbid: Skip if the previous is still running.
- Replace: Kill the previous and start a new one.



 

##### CronJob vs Job:

|**Feature**|**Job**|**CronJob**|
|---|---|---|
|Run once|✅ Yes|❌ No (runs on schedule)|
|Scheduled runs|❌ No|✅ Yes|
|Use case|One-time tasks|Recurring tasks|

##### What is Linux Cron?

**Cron** is a **time-based job scheduler** in Unix/Linux systems.

It allows users to **run commands or scripts automatically at scheduled
times** or intervals.

 

##### Key Features:

- Automates repetitive tasks (e.g., backups, updates, emails).
- Runs in the background via the **crond** daemon.
- Uses **cron expressions** to define schedules.

### ⏱️ Cron Expression Format

```scss
* * * * *
│ │ │ │ │
│ │ │ │ └── Day of the week (0-6) (Sunday=0)
│ │ │ └──── Month (1-12)
│ │ └────── Day of the month (1-31)
│ └──────── Hour (0-23)
└────────── Minute (0-59)
```

> Example: `*/5 * * * *` = Runs every 5 minutes

## 🧾 Commands

```bash
kubectl create -f cronjob.yaml \# Create CronJob\
kubectl get cronjobs \# List CronJobs\
kubectl describe cronjob \<name\> \# View details\
kubectl delete cronjob \<name\> \# Delete CronJob\
kubectl get jobs \--watch \# Watch Job executions
```
