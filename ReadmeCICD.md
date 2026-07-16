# Clean Architecture CI/CD Pipeline using Azure DevOps and AWS EC2

## Project Overview

This project demonstrates the implementation of a complete end-to-end Continuous Integration and Continuous Deployment (CI/CD) pipeline for a .NET 10 Clean Architecture application using Azure DevOps and AWS EC2.

The application follows Microsoft's Clean Architecture principles and consists of an ASP.NET Core Web API with an Angular frontend. The primary objective of this project is to automate the software delivery process, eliminating manual deployment activities while ensuring consistent, reliable, and repeatable deployments.

The CI/CD pipeline is implemented using Azure DevOps YAML pipelines and integrates multiple technologies including GitHub, .NET SDK, Node.js, Azure DevOps Artifacts, SSH Service Connections, and Amazon Linux 2023 running on AWS EC2.

The pipeline automatically performs source code checkout, package restoration, application build, unit testing, Angular build (during the .NET publish process), artifact generation, artifact transfer to AWS EC2, application deployment, backup of the previous version, service restart, and health verification.

This project demonstrates practical DevOps concepts such as pipeline automation, artifact management, deployment automation, Linux service management, application health checks, secure SSH-based deployments, and rollback preparation through deployment backups.

The implementation closely resembles a real-world CI/CD workflow used by many organizations for deploying enterprise applications to Linux-based cloud servers.

The objective of this project is to automate the entire software delivery lifecycle including:

- Source Code Management
- Continuous Integration
- Automated Build
- Unit Testing
- Artifact Publishing
- Continuous Deployment
- Linux Service Management
- Health Verification

---

# Architecture

```
                    GitHub Repository
                           │
                           ▼
                  Azure DevOps Pipeline
                  ---------------------
                  Continuous Integration
                  ---------------------

          Restore Packages
                  │
                  ▼
             Build Solution
                  │
                  ▼
             Execute Unit Tests
                  │
                  ▼
          Publish Application
                  │
                  ▼
          Publish Build Artifact

                           │
                           ▼

                  Continuous Deployment

                  Download Artifact
                           │
                           ▼
             Copy Artifact to AWS EC2
                           │
                           ▼
             Stop Existing Application
                           │
                           ▼
                Backup Previous Build
                           │
                           ▼
               Deploy New Application
                           │
                           ▼
               Restart Application
                           │
                           ▼
                  Health Check
                           │
                           ▼
                Application Available
```

---

# Technology Stack

| Category | Technology |
|-----------|------------|
| Programming Language | C# |
| Framework | .NET 10 |
| Frontend | Angular |
| Architecture | Clean Architecture |
| CI/CD | Azure DevOps |
| Source Control | GitHub |
| Cloud | AWS EC2 |
| Operating System | Amazon Linux 2023 |
| Service Management | systemd |
| Build Tool | MSBuild |
| Package Manager | NuGet |
| Frontend Package Manager | npm |
| Node.js | Version 20 |
| YAML Pipeline | Azure Pipelines |

---

# Repository Structure

```
CleanArchitecture
│
├── src
│   ├── Domain
│   ├── Application
│   ├── Infrastructure
│   ├── Web
│   ├── Shared
│   ├── ServiceDefaults
│   └── AppHost
│
├── tests
│   ├── Application.UnitTests
│   ├── Domain.UnitTests
│   ├── Application.FunctionalTests
│   ├── Infrastructure.IntegrationTests
│   ├── Web.AcceptanceTests
│   └── TestAppHost
│
├── azure-pipelines.yml
│
└── README.md
```

---

# CI/CD Pipeline

The pipeline consists of two stages.

## Stage 1 – Continuous Integration

The CI stage performs the following tasks:

- Checkout source code
- Install .NET SDK
- Install Node.js
- Restore NuGet packages
- Build solution
- Execute Unit Tests
- Publish Application
- Publish Build Artifact

---

## Stage 2 – Continuous Deployment

The deployment stage performs:

- Download Build Artifact
- Copy files to AWS EC2
- Stop existing application
- Backup previous deployment
- Extract latest build
- Configure file permissions
- Start application service
- Verify service status
- Execute health check
- Complete deployment

---

# Pipeline Flow

```
Developer

↓

Git Push

↓

GitHub

↓

Azure DevOps

↓

Restore

↓

Build

↓

Test

↓

Publish

↓

Publish Artifact

↓

Download Artifact

↓

Copy to EC2

↓

Deploy

↓

Health Check

↓

Application Running
```

---

# Build Artifact

The pipeline generates a deployment artifact containing the published application.

```
Web.zip
```

This artifact is copied to the EC2 server and extracted during deployment.

---

# Deployment Directory

Application deployment path

```
/opt/cleanarchitecture
```

Artifact staging path

```
/home/ec2-user/deploy
```

Backup location

```
/opt/backup
```

---

# Service Management

The application is managed using Linux systemd.

```
cleanarchitecture.service
```

Common commands

```bash
sudo systemctl start cleanarchitecture

sudo systemctl stop cleanarchitecture

sudo systemctl restart cleanarchitecture

sudo systemctl status cleanarchitecture
```

---

# Health Check

The deployment verifies application availability using:

```bash
curl -sSf http://localhost:5000 > /dev/null
```

Deployment succeeds only if the application responds successfully.

---

# Security

The deployment uses:

- Azure DevOps Variable Group
- Azure DevOps SSH Service Connection
- Self Hosted Agent
- Secure SSH Authentication

No credentials are stored in source code.

---

# AWS Components

- EC2
- Amazon Linux 2023
- Security Groups
- SSH
- systemd


# Future Enhancements

Potential improvements include:

- Docker containerization
- Kubernetes deployment
- Terraform Infrastructure as Code
- Nginx Reverse Proxy
- HTTPS with SSL
- SonarQube Code Analysis
- Azure Key Vault Integration
- Blue-Green Deployment
- Deployment Approval Gates

---

# Learning Outcomes

This project demonstrates practical experience in:

- Azure DevOps
- YAML Pipelines
- CI/CD Implementation
- AWS EC2 Deployment
- Linux Administration
- GitHub Integration
- Self Hosted Agents
- Artifact Management
- Automated Deployment
- Health Verification
- Application Backup Strategy

---

# Explaining this Project in an Interview

## Project Summary (2–3 Minutes)

I implemented a complete CI/CD pipeline for a .NET 10 Clean Architecture application with an Angular frontend using Azure DevOps and AWS EC2.

The application source code is stored in GitHub, and every commit to the main branch automatically triggers the Azure DevOps pipeline.

The pipeline consists of two stages:

**Continuous Integration**

During the CI stage, the pipeline performs source code checkout, installs the required .NET SDK and Node.js runtime, restores NuGet packages, builds the solution, executes unit tests, publishes the application, and finally generates a deployment artifact.

Since the project contains an Angular frontend, the Angular application is automatically built during the `dotnet publish` process through the Web project configuration, so no separate Angular build stage is required.

After a successful build, the published application is stored as a pipeline artifact.

**Continuous Deployment**

The CD stage downloads the published artifact and securely copies it to an AWS EC2 instance using Azure DevOps SSH Service Connection.

The deployment script then:

- Stops the currently running application
- Creates a backup of the previous deployment
- Cleans the deployment directory
- Extracts the latest application package
- Updates file ownership
- Starts the application using a Linux systemd service
- Verifies that the service is running
- Performs a health check using curl

Only after the health check succeeds is the deployment considered successful.

This implementation removes manual deployment activities and provides a consistent, repeatable, and automated deployment process.

---

## Technologies Used

- Azure DevOps
- YAML Pipelines
- GitHub
- .NET 10
- Angular
- Node.js
- Azure DevOps Artifacts
- AWS EC2
- Amazon Linux 2023
- SSH Service Connection
- systemd
- Git

---

## Challenges Faced

During the implementation I encountered several real-world issues including:

- Self-hosted agent configuration
- Node.js not available during Angular build
- .NET runtime version mismatch
- hostpolicy.dll publish error
- Linux deployment issues
- Port binding issues
- systemd service configuration
- Application accessibility from browser
- SSH deployment automation
- Artifact extraction
- Health check failures
- YAML syntax issues

Each issue was analyzed, troubleshooted, and resolved, providing practical experience with Azure DevOps pipeline debugging and Linux application deployment.

---

## Key Learning Outcomes

Through this project I gained practical experience in:

- Designing CI/CD pipelines
- Azure DevOps YAML pipelines
- Artifact management
- Automated deployment
- AWS EC2 administration
- Linux service management
- SSH-based deployments
- Application health verification
- Pipeline troubleshooting
- Deployment automation

# Author

**Sravani Punnam**

Azure DevOps | AWS | .NET | CI/CD | DevOps
