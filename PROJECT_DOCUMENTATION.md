# PROJECT DOCUMENTATION

# Azure DevOps CI/CD Pipeline for .NET 10 Clean Architecture Application using AWS EC2

---

# 1. Introduction

## 1.1 Project Overview

This project demonstrates the implementation of a complete Continuous Integration and Continuous Deployment (CI/CD) pipeline for a .NET 10 Clean Architecture application using Azure DevOps YAML Pipelines and Amazon Web Services (AWS).

The application follows Microsoft's Clean Architecture principles and consists of an ASP.NET Core Web API integrated with an Angular frontend. The entire software delivery lifecycle has been automated—from source code commit to deployment on an AWS EC2 instance.

The project focuses on implementing industry-standard DevOps practices, including automated builds, unit testing, artifact generation, deployment automation, Linux service management, and application health verification.

---

## 1.2 Project Objectives

The primary objectives of this project are:

- Automate application build and deployment.
- Eliminate manual deployment activities.
- Reduce deployment time and human errors.
- Maintain deployment consistency across environments.
- Implement a reusable CI/CD pipeline.
- Deploy the application automatically to AWS EC2.
- Perform deployment validation using health checks.
- Maintain deployment backups for recovery purposes.

---

# 2. Business Requirement

In many organizations, application deployment is performed manually.

A typical manual deployment process involves:

- Building the application on a developer machine.
- Copying published files manually.
- Connecting to the target server using SSH.
- Stopping the existing application.
- Replacing application files.
- Restarting the application.
- Verifying the deployment manually.

This process is time-consuming, error-prone, and difficult to repeat consistently.

To address these challenges, this project automates the complete deployment workflow using Azure DevOps.

---

# 3. Solution Overview

Whenever a developer pushes code to the GitHub repository, Azure DevOps automatically triggers the pipeline.

The pipeline performs the following operations:

1. Downloads the latest source code.
2. Restores project dependencies.
3. Builds the application.
4. Executes unit tests.
5. Publishes the application.
6. Creates a deployment artifact.
7. Transfers the artifact to AWS EC2.
8. Stops the running application.
9. Creates a backup of the previous deployment.
10. Deploys the latest application.
11. Restarts the application.
12. Performs a health check.
13. Marks the deployment as successful.

This process ensures that every deployment follows the same repeatable and reliable workflow.

---

# 4. Technology Stack

| Category | Technology |
|----------|------------|
| Programming Language | C# |
| Framework | .NET 10 |
| Frontend | Angular |
| Architecture | Clean Architecture |
| Version Control | GitHub |
| CI/CD | Azure DevOps |
| Cloud Platform | AWS |
| Compute | Amazon EC2 |
| Operating System | Amazon Linux 2023 |
| Build Tool | MSBuild |
| Package Manager | NuGet |
| Frontend Package Manager | npm |
| Runtime | Node.js 20 |
| Service Manager | systemd |
| Pipeline | YAML |

---

# 5. Solution Architecture

The following diagram illustrates the complete deployment workflow.

```
                    Developer
                        │
                        ▼
                  GitHub Repository
                        │
                        ▼
             Azure DevOps YAML Pipeline
                        │
        ┌───────────────┴───────────────┐
        │                               │
        ▼                               ▼
 Continuous Integration        Continuous Deployment
        │                               │
 Restore Packages             Download Artifact
        │                               │
 Build Solution            Copy Files to EC2
        │                               │
 Execute Tests          Stop Running Service
        │                               │
 Publish Application      Backup Deployment
        │                               │
 Publish Artifact        Extract New Build
        │                               │
                        Start Service
                               │
                               ▼
                       Health Verification
                               │
                               ▼
                     Application Available
```

---

# 6. Repository Structure

```
CleanArchitecture
│
├── src
│   ├── AppHost
│   ├── Application
│   ├── Domain
│   ├── Infrastructure
│   ├── ServiceDefaults
│   ├── Shared
│   └── Web
│
├── tests
│   ├── Application.FunctionalTests
│   ├── Application.UnitTests
│   ├── Domain.UnitTests
│   ├── Infrastructure.IntegrationTests
│   ├── TestAppHost
│   └── Web.AcceptanceTests
│
├── azure-pipelines.yml
├── CleanArchitecture.slnx
├── README.md
├── PROJECT_DOCUMENTATION.md
├── PIPELINE_EXPLANATION.md
├── TROUBLESHOOTING_GUIDE.md
└── INTERVIEW_GUIDE.md
```

---

# 7. Folder Description

## src

The **src** folder contains the application source code.

### Domain

Contains enterprise business entities and core business rules.

The Domain layer has no dependency on any other project.

---

### Application

Contains application business logic.

Responsibilities include:

- Commands
- Queries
- Validation
- DTOs
- Interfaces

The Application layer depends only on the Domain layer.

---

### Infrastructure

Contains implementation details such as:

- Database access
- Entity Framework Core
- Repository implementations
- External services
- Logging

---

### Web

The Web project acts as the application's entry point.

Responsibilities include:

- ASP.NET Core Web API
- Angular frontend hosting
- API endpoints
- Middleware configuration
- Authentication
- Static file hosting

During the publish process, the Angular application is automatically built and included in the published output.

---

### Shared

Contains reusable components shared across multiple projects.

---

### ServiceDefaults

Contains common service configuration used by the application.

---

### AppHost

Provides application hosting and startup configuration for local development.

---

# 8. Tests Folder

The **tests** folder contains different categories of automated tests.

### Application.UnitTests

Tests individual components of the Application layer in isolation.

---

### Domain.UnitTests

Tests business rules implemented in the Domain layer.

---

### Application.FunctionalTests

Validates application functionality by testing complete workflows.

---

### Infrastructure.IntegrationTests

Verifies Infrastructure components such as database operations and external integrations.

---

### Web.AcceptanceTests

Validates the application from an end-user perspective.

---

### TestAppHost

Provides the hosting environment required for automated testing.
