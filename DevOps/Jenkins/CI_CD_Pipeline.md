# Jenkins – CI_CD_Pipeline

🗓️ **Date:** 2025-07-02  
🕒 **Time:** 12:00  

🏷️ **Tags:** #jenkins #devops #CI_CD_Pipeline  

---

## 📝 Notes

 ##### CI/CD pipeline: aa

- Continuous Integration Continuous Deployment/Delivery
- It’s a methodology
##### Jenkins

- Its an open-source project written in java that runs on windows, mac OS and other UNIX like operating system its free, community supported and might be your first choice tool for CI
- Jenkins automate the entire software life cycle
- It was originally developed by Sun Microsystem in 2004 under the name Hudson the project was later named Jenkins when Oracle bought microsystem it can run on any major platform without any compatibility issues
- Whenever developers write the code we integrate all the code of all developers at that point of time and we build, test and deliver or deploy to the client this process is called CI/CD
- Jenkins helps us to achieve this because of CI, now bugs will be reported fast and get rectified fast. So the entire SDLC happens fast

	Jenkins has a **Master-Slave Architecture**
##### Jenkins Workflow

- We can attach git, Maven and Selenium at Artifactory plugins to Jenkins
- Once developers put code to GitHub, Jenkins pull that code and send to maven for build
- Once build is done Jenkins pull that code and send to selenium for testing
- once testing is done Jenkins pull that code and send to Artifactory as per requirement and so on
- We can also deploy with Jenkins
##### Advantages of Jenkins

- It has lot of plug-ins available
- You can write your own plugin Jenkins is not just a tool it is a framework i.e. you can do whatever you want all you need is plugins
- We can attach slaves to Jenkins master. It instruct others to do jobs. If slaves are not available Jenkins itself do the jobs
- Jenkins also behaves as Cron-server replacement that is it can do scheduled tasks
- It can create labels
## 🧾 Commands

```bash
# Example:
kubectl get pods
```
