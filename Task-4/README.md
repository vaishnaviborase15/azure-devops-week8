# Azure DevOps Pipelines: Creating a Service Connection

## Overview

This repository outlines the process of creating and managing **Service Connections** in Azure DevOps. A Service Connection allows Azure DevOps to securely connect with external services such as Azure, AWS, Docker Hub, GitHub, and others, enabling deployment, integration, and resource management during CI/CD processes.

## Objectives

- Understand the role of service connections in Azure DevOps pipelines.
- Create a service connection for Azure Resource Manager.
- Configure the connection for secure and scoped usage within pipelines.
- Use the service connection in pipeline YAML or classic pipelines.

## Prerequisites

- An Azure DevOps Organization and Project.
- Proper permissions (Project Administrator or Service Connection Administrator).
- Access to the target external service (e.g., Azure subscription).

## Types of Service Connections

Azure DevOps supports various types of service connections, including but not limited to:

- **Azure Resource Manager (ARM)**
- **Docker Registry**
- **GitHub**
- **AWS**
- **Bitbucket Cloud**
- **Generic Service Connection (for REST-based services)**

This README focuses on setting up an **Azure Resource Manager** service connection.

## Steps to Create a Service Connection (Azure Resource Manager)

### Option 1: Use the Azure DevOps Portal

1. Go to your Azure DevOps project.
2. Navigate to **Project Settings > Service Connections**.
3. Click **+ New service connection**.
4. Select **Azure Resource Manager**.
5. Choose one of the authentication methods:
   - **Service principal (automatic)** (recommended for most use cases)
   - **Service principal (manual)** (requires manual entry of app ID, secret, and tenant)
6. Select your Azure subscription and authorize.
7. Provide a **Service connection name** (e.g., `MyAzureConnection`) and optionally restrict access to pipelines.
8. Click **Verify and Save**.

### Option 2: Manual Service Principal Entry

1. Register an application in **Azure Active Directory > App registrations**.
2. Generate a client secret.
3. Assign **Contributor** role to the app in the Azure Subscription or Resource Group.
4. In Azure DevOps, create a new service connection with:
   - Client ID (Application ID)
   - Client Secret
   - Tenant ID
   - Subscription ID and Name

## Usage in YAML Pipelines

```yaml
trigger:
- main

pool:
  vmImage: 'ubuntu-latest'

steps:
- task: AzureCLI@2
  inputs:
    azureSubscription: 'MyAzureConnection'  # Name of the service connection
    scriptType: 'bash'
    scriptLocation: 'inlineScript'
    inlineScript: |
      az group list
```

## Best Practices

- Use naming conventions for service connections (e.g., `azure-dev-connection`, `prod-dockerhub`).
- Restrict access to service connections to only required pipelines or users.
- Regularly review permissions and rotate secrets if using manual connections.
- Use environment variables or variable groups to pass credentials securely.

## Resources

- [Create and Use Service Connections in Azure DevOps (Microsoft Docs)](https://learn.microsoft.com/en-us/azure/devops/pipelines/library/service-endpoints?view=azure-devops)
- [Video: Azure DevOps Test Plans & Service Connections](https://www.youtube.com/watch?v=Cu7zx9u1sOE)

## Author

**Vaishnavi Borase**  
B.Tech CSE (Data Science), R. C. Patel Institute of Technology  
CSI ID: **CT_CSI_DV_4845**  
Email: **221106014@rcpit.ac.in**

