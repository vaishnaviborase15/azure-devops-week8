# CI/CD Pipeline: Build and Push Docker Image to ACR & Deploy to ACI Using Azure DevOps

## Overview

This guide demonstrates how to build a CI/CD pipeline in Azure DevOps that automates:

1. Building a Docker image from application source code.
2. Pushing the Docker image to **Azure Container Registry (ACR)**.
3. Deploying the Docker image to **Azure Container Instances (ACI)**.

This pipeline streamlines continuous integration and continuous deployment for containerized applications using Azure's managed container services.

## Objectives

- Build Docker images in a CI pipeline.
- Push images securely to Azure Container Registry.
- Deploy containerized apps to Azure Container Instances.
- Automate deployment with Azure CLI commands in pipelines.

## Prerequisites

- Azure DevOps organization and project.
- Azure subscription with:
  - Azure Container Registry (ACR) created.
  - Azure Container Instances enabled.
- Azure DevOps service connection with permission to access Azure resources.
- Application repository with a Dockerfile.

## Pipeline Stages

### 1. Build and Push Docker Image

- Use the Docker task to build the image from your Dockerfile.
- Push the built image to your ACR repository.
- Tag images uniquely (e.g., using build ID).

### 2. Deploy to Azure Container Instances

- Use Azure CLI task to deploy or update the container instance.
- Pull the image from ACR.
- Configure container instance properties such as CPU, memory, ports, and environment variables.

## Sample Azure DevOps YAML Pipeline

```yaml
trigger:
- main

variables:
  azureSubscription: 'MyAzureServiceConnection'
  acrName: 'myacr.azurecr.io'
  imageName: 'myapp'
  resourceGroup: 'myResourceGroup'
  containerGroupName: 'myContainerGroup'
  containerName: 'mycontainer'
  location: 'eastus'

stages:
- stage: BuildAndPush
  jobs:
  - job: BuildPushJob
    pool:
      vmImage: 'ubuntu-latest'
    steps:
    - task: Docker@2
      displayName: Build and Push Docker Image
      inputs:
        containerRegistry: $(azureSubscription)
        repository: $(acrName)/$(imageName)
        command: buildAndPush
        Dockerfile: '**/Dockerfile'
        tags: |
          $(Build.BuildId)

- stage: Deploy
  dependsOn: BuildAndPush
  jobs:
  - deployment: DeployToACI
    environment: 'aci-environment'
    pool:
      vmImage: 'ubuntu-latest'
    strategy:
      runOnce:
        deploy:
          steps:
          - task: AzureCLI@2
            inputs:
              azureSubscription: $(azureSubscription)
              scriptType: bash
              scriptLocation: inlineScript
              inlineScript: |
                az container create \
                  --resource-group $(resourceGroup) \
                  --name $(containerGroupName) \
                  --image $(acrName)/$(imageName):$(Build.BuildId) \
                  --cpu 1 --memory 1.5 \
                  --registry-login-server $(acrName) \
                  --registry-username $(acrName) \
                  --registry-password $(AZURE_ACR_PASSWORD) \
                  --dns-name-label $(containerGroupName) \
                  --ports 80
```

> **Note:** Make sure to securely store your Azure Container Registry password (or use service principal authentication) and reference it in the pipeline as a secret variable (e.g., `AZURE_ACR_PASSWORD`).

## Best Practices

- Use service connections with least privilege principle.
- Securely manage credentials and secrets using Azure DevOps Library or Azure Key Vault.
- Tag Docker images uniquely to enable traceability.
- Monitor container instance health and logs regularly.
- Clean up unused container instances to avoid unnecessary costs.

## Resources

- [Azure DevOps Pipelines Documentation](https://learn.microsoft.com/en-us/azure/devops/pipelines/?view=azure-devops)
- [Deploy Azure Container Instances using Azure CLI](https://learn.microsoft.com/en-us/azure/container-instances/container-instances-quickstart)
- [Azure DevOps CI/CD Video Tutorial](https://www.youtube.com/watch?v=o9OpFMQMSHw)

## Authors

**Vaishnavi Borase**  
B.Tech CSE (Data Science), R. C. Patel Institute of Technology  
CSI ID: CT_CSI_DV_4845  
Email: 221106014@rcpit.ac.in  

