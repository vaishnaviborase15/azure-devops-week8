# Azure DevOps: Creating a Service Connection

## Overview

Service Connections in Azure DevOps provide secure authentication and authorization to external services such as Azure, Docker registries, GitHub, and more. This enables pipelines to deploy, manage resources, and interact with these services seamlessly.

This guide walks you through creating a Service Connection, focusing primarily on Azure Resource Manager (ARM) connections, which are commonly used for deploying Azure resources.

## Objectives

- Understand the concept and importance of service connections.
- Create a service connection in Azure DevOps to authenticate against Azure.
- Use the service connection securely within pipelines.
- Manage service connection permissions and scopes.

## Prerequisites

- Access to an Azure DevOps organization and project.
- Required permissions (Project Administrator or Service Connection Administrator).
- An active Azure subscription.
- A Personal Access Token (PAT) with appropriate permissions if needed.

## Types of Service Connections

Azure DevOps supports various service connections, including but not limited to:

- Azure Resource Manager
- Docker Registry
- GitHub
- AWS
- Bitbucket Cloud
- Generic service connections for custom endpoints

## Steps to Create an Azure Resource Manager Service Connection

### Via Azure DevOps Portal

1. Navigate to your Azure DevOps project.
2. Go to **Project Settings** (bottom-left corner).
3. Click on **Service connections** under **Pipelines**.
4. Click on **New service connection**.
5. Select **Azure Resource Manager** and click **Next**.
6. Choose an authentication method:
   - **Service principal (automatic)** — Recommended for most users; Azure DevOps creates the service principal.
   - **Service principal (manual)** — Requires manual details like client ID, secret, tenant, and subscription.
7. Select your Azure subscription from the dropdown.
8. Provide a connection name (e.g., `MyAzureConnection`).
9. Optionally restrict the connection access to specific pipelines.
10. Click **Verify and save**.

### Manual Service Principal Creation (Optional)

If you want to create the service principal manually:

- Register an application in Azure Active Directory.
- Generate a client secret.
- Assign the service principal the appropriate role in your Azure subscription or resource group (usually Contributor).
- Enter the service principal details during the service connection setup.

## Using Service Connection in Pipelines

Reference your service connection in pipeline YAML tasks, e.g.,

```yaml
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

- Use descriptive and consistent naming for service connections.
- Limit permissions of service principals to the minimum required.
- Regularly review and rotate secrets.
- Restrict service connection access to authorized pipelines and users.
- Document service connections and their purposes clearly.

## Resources

- [Azure DevOps Service Connections Documentation](https://learn.microsoft.com/en-us/azure/devops/pipelines/library/service-endpoints?view=azure-devops)
- [Azure DevOps Service Connections Video Tutorial](https://www.youtube.com/watch?v=Cu7zx9u1sOE)

## Author

**Akash Shinde**  
B.Tech CSE (Data Science), R. C. Patel Institute of Technology  
CSI ID: **CT_CSI_DV_4920**  
Email: **221106045@rcpit.ac.in**

