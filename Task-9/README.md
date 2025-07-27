
##  CI/CD Pipeline to Build a .NET Application and Deploy to Azure App Service using Azure DevOps

###  Overview

This project demonstrates how to set up a CI/CD pipeline using Azure DevOps to automate building a .NET application and deploying it to **Azure App Service**. The setup ensures reliable and repeatable deployments of your application with minimal manual intervention.

---

###  Tools & Technologies Used

* **Azure DevOps Pipelines**
* **.NET Core Application**
* **Azure App Service**
* **GitHub / Azure Repos**
* **YAML**
* **Azure Resource Manager (ARM)**

---

###  Project Structure

```
dotnet-app/
├── Controllers/
├── Views/
├── Program.cs
├── Startup.cs
├── dotnet-app.csproj
└── azure-pipelines.yml
```

---

###  Azure Resources Used

* Azure Resource Group
* Azure App Service Plan
* Azure Web App (.NET)
* Azure DevOps Service Connection

---

###  CI/CD Flow

#### Continuous Integration (CI)

* Triggered on each commit to `main` branch
* Restores packages
* Builds the .NET solution
* Publishes build artifacts

#### Continuous Deployment (CD)

* Deploys build artifact to Azure Web App
* Uses AzureWebApp task with service connection

---

###  `azure-pipelines.yml`

```yaml
trigger:
- main

pool:
  vmImage: 'windows-latest'

variables:
  buildConfiguration: 'Release'
  azureSubscription: 'YourServiceConnectionName'
  webAppName: 'your-webapp-name'

steps:
- task: UseDotNet@2
  inputs:
    packageType: 'sdk'
    version: '7.x.x'

- task: DotNetCoreCLI@2
  inputs:
    command: 'restore'
    projects: '**/*.csproj'

- task: DotNetCoreCLI@2
  inputs:
    command: 'build'
    projects: '**/*.csproj'
    arguments: '--configuration $(buildConfiguration)'

- task: DotNetCoreCLI@2
  inputs:
    command: 'publish'
    publishWebProjects: true
    arguments: '--configuration $(buildConfiguration) --output $(Build.ArtifactStagingDirectory)'
    zipAfterPublish: true

- task: PublishBuildArtifacts@1
  inputs:
    pathToPublish: '$(Build.ArtifactStagingDirectory)'
    artifactName: 'drop'
    publishLocation: 'Container'

- task: AzureWebApp@1
  inputs:
    azureSubscription: '$(azureSubscription)'
    appName: '$(webAppName)'
    package: '$(Pipeline.Workspace)/drop/**/*.zip'
```

---

###  Output

* .NET application is built and zipped
* Artifact is published
* App is deployed to Azure App Service automatically

---

###  Authors

**Akash Shnde**
B.Tech CSE (Data Science), R. C. Patel Institute of Technology
CSI ID: **CT\_CSI\_DV\_4920**
Email: **[221106045@rcpit.ac.in](mailto:221106045@rcpit.ac.in)**


