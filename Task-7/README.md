# CI/CD Pipeline: Build and Push Docker Image to ACR & Deploy to AKS Using Azure DevOps

## Overview

This project demonstrates how to build a robust **CI/CD pipeline** using Azure DevOps that automates the following workflow:

1. Build a Docker image from application source code.
2. Push the Docker image to **Azure Container Registry (ACR)**.
3. Deploy the Docker image to an **Azure Kubernetes Service (AKS)** cluster.

This pipeline enables continuous integration and continuous delivery, ensuring rapid, consistent, and reliable application deployment on Kubernetes.

## Objectives

- Configure Azure DevOps pipeline to automate Docker image build and push.
- Authenticate securely with Azure Container Registry.
- Deploy or update applications running on Azure Kubernetes Service.
- Implement best practices for security, scalability, and maintainability.

## Prerequisites

- Azure DevOps Organization and Project.
- Azure subscription with:
  - Azure Container Registry (ACR) instance.
  - Azure Kubernetes Service (AKS) cluster configured.
- Service Connection in Azure DevOps for Azure Resource Manager with permissions on ACR and AKS.
- Source code repository with a Dockerfile for the application.

## Pipeline Overview

The pipeline consists of the following stages:

### 1. Build Stage
- Build a Docker image using the Dockerfile in the repository.
- Tag the image appropriately (e.g., with build ID or commit SHA).

### 2. Push Stage
- Authenticate to Azure Container Registry.
- Push the tagged image to ACR.

### 3. Deploy Stage
- Use `kubectl` or Helm to deploy/update the application in the AKS cluster.
- Pull the image from ACR and update the Kubernetes deployment.

## Sample YAML Pipeline Snippet

```yaml
trigger:
- main

variables:
  azureSubscription: 'MyAzureServiceConnection'
  acrName: 'myacr.azurecr.io'
  imageName: 'myapp'
  kubernetesNamespace: 'default'
  aksCluster: 'myAKSCluster'
  resourceGroup: 'myResourceGroup'

stages:
- stage: Build
  jobs:
  - job: BuildAndPush
    pool:
      vmImage: 'ubuntu-latest'
    steps:
    - task: Docker@2
      displayName: Build Docker Image
      inputs:
        containerRegistry: $(azureSubscription)
        repository: $(acrName)/$(imageName)
        command: build
        Dockerfile: '**/Dockerfile'
        tags: |
          $(Build.BuildId)

    - task: Docker@2
      displayName: Push Docker Image
      inputs:
        containerRegistry: $(azureSubscription)
        repository: $(acrName)/$(imageName)
        command: push
        tags: |
          $(Build.BuildId)

- stage: Deploy
  dependsOn: Build
  jobs:
  - deployment: DeployToAKS
    environment: 'aks-environment'
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
                az aks get-credentials --resource-group $(resourceGroup) --name $(aksCluster)
                kubectl set image deployment/myapp-deployment myapp=$(acrName)/$(imageName):$(Build.BuildId) -n $(kubernetesNamespace)
```

## Best Practices

- Use service connections with least privilege access.
- Tag Docker images uniquely using build numbers or commit hashes.
- Store secrets and sensitive info in Azure DevOps Library or Key Vault.
- Use Kubernetes rolling updates for zero downtime deployment.
- Monitor deployments and set up alerts on failure.

## Useful Resources

- [Azure DevOps Pipelines Documentation](https://learn.microsoft.com/en-us/azure/devops/pipelines/?view=azure-devops)
- [Official Azure DevOps Docker Tasks](https://learn.microsoft.com/en-us/azure/devops/pipelines/tasks/build/docker)
- [Deploy to AKS using kubectl](https://learn.microsoft.com/en-us/azure/aks/kubernetes-deploy-cli)
- [Azure DevOps CI/CD Video Tutorial](https://www.youtube.com/watch?v=o9OpFMQMSHw)

## Authors

**Vaishnavi Borase**  
B.Tech CSE (Data Science), R. C. Patel Institute of Technology  
CSI ID: CT_CSI_DV_4845  
Email: 221106014@rcpit.ac.in  

