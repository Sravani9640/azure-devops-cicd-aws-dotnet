INTERVIEW GUIDE
Project Summary (30–60 Seconds)

"I implemented an end-to-end CI/CD pipeline for a .NET 10 Clean Architecture application with an Angular frontend using Azure DevOps and AWS EC2.

The source code is stored in GitHub. Whenever code is pushed to the main branch, Azure DevOps automatically triggers the pipeline. During the CI stage, the pipeline checks out the code, restores NuGet packages, installs Node.js for the Angular build, builds the solution, executes unit tests, publishes the application, and creates a build artifact.

In the CD stage, the artifact is securely copied to an AWS EC2 instance over SSH. The deployment script stops the existing application, creates a backup, extracts the latest build, starts the application using a Linux systemd service, and performs a health check to verify a successful deployment."

2-Minute Project Explanation

"The objective of this project was to automate application deployment and eliminate manual deployment steps.

I used GitHub as the source code repository and Azure DevOps YAML pipelines to implement Continuous Integration and Continuous Deployment.

During Continuous Integration, the pipeline restores dependencies, compiles the solution, executes unit tests, publishes the application, and stores the published output as an artifact.

During Continuous Deployment, Azure DevOps downloads the artifact, copies it to an AWS EC2 instance using an SSH Service Connection, extracts the deployment package, starts the application using systemd, and verifies the deployment with a health check.

I also configured a self-hosted Azure DevOps agent, created Azure DevOps Variable Groups for configuration, and automated the deployment process using Linux shell commands."

5-Minute Project Explanation

Explain the project in this order:

1. Project Overview
.NET 10 Clean Architecture
Angular frontend
Azure DevOps
GitHub
AWS EC2
Amazon Linux
2. Architecture
Developer

↓

GitHub Repository

↓

Azure DevOps Pipeline

↓

CI Stage

↓

Artifact

↓

CD Stage

↓

AWS EC2

↓

Application Running
3. CI Stage

Explain every task.

Checkout

↓

UseDotNet

↓

UseNode

↓

Restore

↓

Build

↓

Unit Test

↓

Publish

↓

Publish Artifact

4. CD Stage

Explain every task.

Download Artifact

↓

CopyFilesOverSSH

↓

SSH

↓

Stop Service

↓

Backup

↓

Clean Directory

↓

Unzip

↓

Start Service

↓

Health Check

5. Result
Fully automated deployment
No manual deployment
Repeatable releases
Faster deployments
Reliable deployments
End-to-End Pipeline Flow
Developer

↓

Git Push

↓

GitHub

↓

Azure DevOps Trigger

↓

Checkout

↓

Restore Packages

↓

Build Solution

↓

Run Tests

↓

Publish Application

↓

Create Artifact

↓

Download Artifact

↓

Copy Artifact to EC2

↓

Stop Existing Service

↓

Backup Existing Files

↓

Deploy Latest Files

↓

Start Service

↓

Health Check

↓

Deployment Complete
Interview Questions & Answers
1. Why did you use Azure DevOps?

Answer

Azure DevOps provides source integration, build automation, release automation, artifact management, environments, approvals, and YAML pipelines in a single platform.

2. Why YAML instead of Classic Pipeline?

Answer

YAML pipelines are version-controlled, reusable, easier to maintain, and support Infrastructure as Code practices.

3. Why UseDotNet?

Answer

To install the required .NET SDK version on the build agent before building the application.

4. Why UseNode?

Answer

The project contains an Angular frontend. During the publish process, Angular is built automatically, so Node.js is required.

5. Why Restore?

Answer

Restore downloads all NuGet packages required for compilation.

6. Why Build?

Answer

Build compiles the application and verifies there are no compilation errors.

7. Why Run Tests?

Answer

Unit tests validate application functionality before deployment.

8. Why Publish?

Answer

Publish creates deployable application files.

9. Why Publish Build Artifact?

Answer

The deployment stage should use the exact build generated during CI instead of rebuilding.

10. Why Download Artifact?

Answer

The CD stage retrieves the published artifact created by the CI stage.

11. Why CopyFilesOverSSH?

Answer

It securely transfers the deployment package from Azure DevOps to AWS EC2.

12. Why SSH Task?

Answer

To execute Linux deployment commands remotely on the EC2 instance.

13. Why Backup Before Deployment?

Answer

To enable rollback if the latest deployment fails.

14. Why systemd?

Answer

systemd ensures:

Automatic restart
Automatic startup after reboot
Service monitoring
Centralized logging
15. Why Health Check?

Answer

To confirm that the application is running successfully after deployment.

16. Why Self-Hosted Agent?

Answer

My Azure DevOps organization didn't have Microsoft-hosted parallelism available, so I configured a self-hosted agent to run the pipeline.

17. Why AWS EC2?

Answer

EC2 provides a Linux server where the application can be deployed and managed, simulating a real production environment.

18. What challenges did you face?

You can mention:

Self-hosted agent configuration
YAML indentation errors
Node.js installation
.NET runtime mismatch
localhost vs 0.0.0.0 binding
Port 5000 accessibility
systemd service creation
Health check failures
SSH deployment issues
Artifact extraction
19. What did you learn?
Azure DevOps YAML
CI/CD implementation
AWS EC2 deployment
Linux administration
systemd
Azure DevOps Variable Groups
Artifact management
Troubleshooting production deployment issues
20. What improvements would you make?
Docker
Kubernetes
Terraform
Nginx reverse proxy
HTTPS
SonarQube
Azure Key Vault
Blue-Green Deployment
Monitoring
Automated Rollback
Final Interview Closing Statement

"This project helped me gain practical experience in designing and implementing an end-to-end CI/CD pipeline using Azure DevOps. Beyond creating the pipeline, I resolved real deployment challenges related to self-hosted agents, Linux services, runtime dependencies, networking, and health checks. Through this project, I strengthened my understanding of DevOps practices and built confidence in automating application deployments to AWS."

Repository Structure

Your GitHub repository can now look like this:

azure-devops-cicd-aws-dotnet/
│
├── src/
├── tests/
├── azure-pipelines.yml
├── README.md
├── PROJECT_DOCUMENTATION.md
├── TROUBLESHOOTING_GUIDE.md
├── INTERVIEW_GUIDE.md
└── .gitignore

This gives you a repository that is not only functional but also well-documented and interview-ready.


Project Overview
Tell me about your CI/CD project.
What problem were you trying to solve with this project?
Why did you choose Azure DevOps?
Why did you choose AWS EC2 for deployment?
Why did you use GitHub instead of Azure Repos?
What is Clean Architecture?
Why did you choose this project?
Explain your project architecture.
Walk me through your deployment flow.
Which technologies did you use in this project?
Git & GitHub
What branching strategy did you use?
What happens after git push?
How does GitHub trigger Azure DevOps?
What is the purpose of the main branch?
How do you resolve merge conflicts?
What is a pull request?
What is git fetch?
What is git pull?
Difference between git merge and git rebase?
How do you revert a bad commit?
Azure DevOps
What are Azure DevOps Pipelines?
What are the different Azure DevOps services?
What is YAML?
Why use YAML pipelines?
Difference between YAML and Classic pipelines?
What is a pipeline trigger?
What is a stage?
What is a job?
What is a deployment job?
What is a step?
What is a task?
What is an agent?
Difference between Microsoft-hosted and Self-hosted agents?
Why did you use a self-hosted agent?
How did you configure your self-hosted agent?
How do you restart an Azure DevOps agent?
What happens if the agent goes offline?
What are deployment environments?
Why did you create a Production environment?
What are approvals in Azure DevOps?
Pipeline Variables
What are pipeline variables?
What is a Variable Group?
Why did you use Variable Groups?
Difference between variables and parameters?
What variables did you store?
How do you secure sensitive values?
What is the difference between runtime and compile-time variables?
CI Stage
Explain your CI stage.
Why is checkout required?
Why UseDotNet task?
Why UseNode task?
Why Restore Packages?
Why Build?
Why Run Tests?
Why Publish?
Why Publish Build Artifacts?
What happens if the build fails?
What happens if tests fail?
Does deployment happen if CI fails?
Why separate Build and Publish?
Angular
Why didn't you add Angular build steps?
How was Angular getting built automatically?
Why did you install Node.js?
What happens if Node.js is missing?
What is npm?
What is package.json?
What is ng build?
What is npm install?
What is generate-api?
How did Angular integrate with .NET?
.NET
Difference between build and publish?
What is dotnet restore?
What is dotnet build?
What is dotnet publish?
Why publish instead of copying binaries?
What are NuGet packages?
What is a csproj file?
What is a solution file?
What is ASP.NET Core Runtime?
Why did you install runtime on EC2?
Artifacts
What is an artifact?
Why create artifacts?
Where are artifacts stored?
Why not build again in CD?
What artifact did your pipeline generate?
What was inside Web.zip?
Difference between Build Artifact and Pipeline Artifact?
AWS EC2
Why EC2?
Which operating system did you use?
What instance type did you use?
What is an Elastic IP?
What are Security Groups?
Why open port 5000?
Why open port 22?
Difference between Security Group and Network ACL?
How do you connect to EC2?
What is SSH?
What authentication method did you use?
What directory did you deploy to?
Why /opt/cleanarchitecture?
Linux
What is systemd?
Why create a service?
What is systemctl?
Difference between start, stop and restart?
What is journalctl?
How did you check logs?
What is chmod?
What is chown?
What is mkdir -p?
What is rm -rf?
What is unzip?
What is curl?
What is ss -tulpn?
What is localhost?
What is 0.0.0.0?
Deployment
Explain your CD stage.
Why CopyFilesOverSSH?
Why SSH task?
Why stop the service before deployment?
Why create a backup?
Why clean the deployment directory?
Why unzip the artifact?
Why change ownership?
Why wait before health check?
What happens during health check?
Why use curl?
What happens if deployment fails?
Troubleshooting
What was the biggest challenge?
Why was your application listening only on localhost?
How did you solve it?
Why did .NET Runtime error occur?
How did you fix runtime mismatch?
Why did your health check fail?
Why did YAML fail initially?
Why did your pipeline fail because of Node.js?
Why did CD fail even when the application worked?
How did you troubleshoot SSH failures?
How did you troubleshoot port issues?
How did you debug Linux services?
What commands did you use most frequently?
Security
How did you secure SSH access?
Why use Service Connections?
How are secrets stored?
Why shouldn't secrets be stored in YAML?
What is least privilege access?
Best Practices
What DevOps best practices did you implement?
Why automate deployments?
Why create backups?
Why use health checks?
Why use version control?
Why use Infrastructure as Code?
How would you improve this pipeline?
Future Enhancements
How would you deploy using Docker?
How would Kubernetes improve this project?
Where would Terraform fit?
How would you add SonarQube?
How would you implement Blue-Green Deployment?
How would you implement Rolling Deployment?
How would you add monitoring?
How would you implement automatic rollback?
Scenario-Based Questions
Build succeeds but deployment fails. How do you troubleshoot?
Tests fail after a recent commit. What do you do?
EC2 is unreachable. What do you check first?
Artifact is missing in CD. How do you investigate?
The application starts locally but not on EC2. What steps would you take?
A new deployment broke the application. How would you roll back?
The self-hosted agent is offline. What would you check?
Users report a 502 Bad Gateway after deployment. What are the possible causes?
The pipeline suddenly becomes much slower. How would you analyze it?
A deployment succeeded, but users still see the old version. What could cause that?

These questions closely match the technologies and implementation details in your project and should prepare you well for Azure DevOps/DevOps interviews.
