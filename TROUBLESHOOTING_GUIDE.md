# TROUBLESHOOTING GUIDE

# Azure DevOps CI/CD Pipeline Troubleshooting

---

# Introduction

This document captures the real-world issues encountered while implementing the CI/CD pipeline for the Clean Architecture application using Azure DevOps and AWS EC2.

Rather than documenting only the successful implementation, this guide records the problems faced, how they were investigated, and how they were resolved.

This serves as both a learning reference and a troubleshooting guide for future deployments.

---

# Issue 1

## Azure DevOps Pipeline Could Not Start

### Problem

The pipeline failed immediately with an error related to Microsoft-hosted agents.

Example:

```
No hosted parallelism has been purchased or granted.
```

---

### Root Cause

The Azure DevOps organization did not have Microsoft-hosted parallelism available.

---

### Investigation

Verified:

- Pipeline configuration
- Agent Pool
- Azure DevOps Organization settings

The issue was not related to YAML.

---

### Resolution

Configured a Self-Hosted Agent on a Windows machine.

Registered the agent with Azure DevOps.

Started the agent service.

Changed the pipeline pool to:

```yaml
pool:
  name: Default
```

---

### Lesson Learned

A pipeline cannot execute without an available agent.

---

# Issue 2

## YAML Indentation Error

### Problem

Pipeline failed before execution.

Example:

```
While scanning a simple key,
could not find expected ':'
```

---

### Root Cause

Incorrect YAML indentation.

---

### Investigation

Compared indentation with Azure DevOps YAML documentation.

---

### Resolution

Corrected indentation.

Maintained consistent spacing throughout the YAML file.

---

### Lesson Learned

YAML is indentation-sensitive.

Even a single missing space can prevent pipeline execution.

---

# Issue 3

## Node.js Not Found

### Problem

Publish step failed.

Example:

```
npm is not recognized
```

or

```
node command not found
```

---

### Root Cause

Angular build requires Node.js.

The build agent did not have Node installed.

---

### Investigation

Verified:

```
node -v

npm -v
```

Commands were unavailable.

---

### Resolution

Installed Node.js in the pipeline.

```yaml
UseNode@1
```

---

### Lesson Learned

Even though the project is primarily .NET, Angular requires Node.js during publishing.

---

# Issue 4

## Angular Build Failure

### Problem

Initially it appeared that Angular was not being built because there were no Angular tasks in the pipeline.

---

### Investigation

Reviewed the publish logs.

Observed:

```
npm install

ng build
```

being executed automatically.

---

### Root Cause

The Angular build is integrated into the .NET publish process through the Web project configuration.

---

### Resolution

No separate Angular pipeline tasks were required.

Only Node.js installation was necessary.

---

### Lesson Learned

Always understand the project structure before adding unnecessary pipeline tasks.

---

# Issue 5

## Artifact Successfully Generated But Not Available on EC2

### Problem

CI succeeded.

Deployment completed.

Application was not updated.

---

### Investigation

Checked:

```
/home/ec2-user/deploy
```

Verified:

```
Web.zip
```

was copied successfully.

---

### Root Cause

Artifact was copied but not extracted.

---

### Resolution

Added:

```bash
unzip -o Web.zip
```

during deployment.

---

### Lesson Learned

Copying artifacts alone is insufficient.

Deployment requires extraction and placement in the application directory.

---

# Issue 6

## .NET Runtime Missing

### Problem

Application failed to start.

Example:

```
You must install or update .NET
```

---

### Root Cause

Only the SDK was available during build.

The EC2 server did not contain the required ASP.NET Runtime.

---

### Investigation

Executed:

```
dotnet --list-runtimes
```

The required runtime was missing.

---

### Resolution

Installed the ASP.NET Core Runtime on Amazon Linux.

Restarted the application.

---

### Lesson Learned

Publishing and hosting require different runtime components.

---

# Issue 7

## Application Running But Not Accessible

### Problem

The service was active.

The application responded locally.

The public EC2 IP did not work.

---

### Investigation

Executed:

```
ss -tulpn
```

Output showed:

```
127.0.0.1:5000
```

---

### Root Cause

The application was listening only on localhost.

---

### Resolution

Updated the systemd service:

```
Environment=ASPNETCORE_URLS=http://0.0.0.0:5000
```

Reloaded systemd.

Restarted the service.

---

### Lesson Learned

Applications deployed to remote servers should listen on all interfaces unless intentionally restricted.

---

# Issue 8

## Address Already In Use

### Problem

Application failed with:

```
Address already in use
```

---

### Root Cause

Another instance of the application was already running on port 5000.

---

### Investigation

Executed:

```
ss -tulpn
```

Identified the running process.

---

### Resolution

Stopped the existing process.

Restarted the service.

---

### Lesson Learned

Always stop existing services before deployment.

---

# Issue 9

## Health Check Failed

### Problem

Deployment stage failed.

Health check returned an error.

---

### Investigation

Executed:

```
curl http://localhost:5000
```

Observed HTML response.

The application was healthy.

---

### Root Cause

The health check command returned a non-zero exit code even though the application was running.

---

### Resolution

Modified the deployment validation logic.

Verified the application response correctly.

---

### Lesson Learned

Always verify the actual application response before assuming deployment failure.

---

# Issue 10

## EC2 Public IP Not Accessible

### Problem

Application was running.

Localhost worked.

Public IP did not.

---

### Investigation

Verified:

AWS Security Group

Inbound Rules

Listening Ports

---

### Root Cause

Port 5000 was not open in the Security Group.

---

### Resolution

Added an inbound rule.

```
Custom TCP

Port 5000

Source

0.0.0.0/0
```

---

### Lesson Learned

Application availability depends on both the application and cloud network configuration.

---

# Issue 11

## SSH Deployment Failed

### Problem

Azure DevOps could not connect to EC2.

---

### Investigation

Verified:

SSH Key

Security Group

Username

Port

Service Connection

---

### Resolution

Created a proper Azure DevOps SSH Service Connection using the EC2 private key.

---

### Lesson Learned

Successful deployment requires both network connectivity and correct authentication.

---

# Issue 12

## systemd Service Configuration

### Problem

The application stopped after closing the SSH session.

---

### Root Cause

The application was started manually.

---

### Resolution

Created:

```
cleanarchitecture.service
```

Configured:

```
systemctl enable

systemctl start
```

---

### Lesson Learned

Production applications should always run as Linux services instead of interactive processes.

---

# Overall Learning

During this project, practical experience was gained in:

- Azure DevOps YAML Pipelines
- Self-Hosted Agents
- GitHub Integration
- AWS EC2
- Amazon Linux
- ASP.NET Core Deployment
- Angular Build Integration
- Linux Services
- SSH
- Application Publishing
- Runtime Installation
- Health Checks
- Deployment Automation
- Backup Strategy
- YAML Troubleshooting
- Pipeline Debugging

---

# Conclusion

Every issue encountered during implementation strengthened the understanding of Azure DevOps, AWS, Linux administration, and CI/CD automation.

Troubleshooting real deployment problems provided valuable hands-on experience that goes beyond simply creating a successful pipeline.

The lessons learned from these issues can be applied directly to future enterprise DevOps projects.
