
##  CI/CD Pipeline to Build a React Application and Deploy to Azure Virtual Machine using Azure DevOps

###  Overview

This project demonstrates how to automate the building and deployment of a **React.js** application to an **Azure Virtual Machine (VM)** using **Azure DevOps Pipelines**. It uses secure connection over SSH to copy the build files to the VM and run deployment scripts remotely.

---

### Tools & Technologies Used

* **React.js**
* **Azure DevOps Pipelines**
* **Azure Virtual Machine (Ubuntu/Windows)**
* **YAML**
* **SSH Deployment**
* **Nginx/Apache** (for hosting React app)

---

###  Project Structure

```
react-app/
├── public/
├── src/
├── .gitignore
├── package.json
├── README.md
└── azure-pipelines.yml
```

---

###  Azure Resources Used

* Azure Virtual Machine (Linux)
* Azure DevOps Pipeline
* SSH Key Pair
* Azure DevOps Service Connection (SSH)

---

###  CI/CD Flow

#### Continuous Integration (CI)

* Triggered on commits to `main` branch
* Installs dependencies
* Builds the React app using `npm run build`
* Publishes build artifacts

#### Continuous Deployment (CD)

* Copies `build` folder to Azure VM via `scp`
* Connects using `SSH` to restart or configure web server (like Nginx)

---

###  `azure-pipelines.yml`

```yaml
trigger:
- main

pool:
  vmImage: 'ubuntu-latest'

variables:
  vmHost: 'your.vm.ip.address'
  vmUsername: 'azureuser'
  vmSSHKey: '$(privateSSHKey)'

steps:
- task: NodeTool@0
  inputs:
    versionSpec: '18.x'
  displayName: 'Install Node.js'

- script: |
    npm install
    npm run build
  displayName: 'Install & Build React App'

- task: CopyFiles@2
  inputs:
    SourceFolder: '$(System.DefaultWorkingDirectory)/build'
    Contents: '**'
    TargetFolder: '$(Build.ArtifactStagingDirectory)/build'

- task: PublishBuildArtifacts@1
  inputs:
    PathtoPublish: '$(Build.ArtifactStagingDirectory)/build'
    ArtifactName: 'drop'
    publishLocation: 'Container'

- task: Bash@3
  inputs:
    targetType: 'inline'
    script: |
      echo "Deploying to Azure VM..."
      scp -o StrictHostKeyChecking=no -i ~/.ssh/id_rsa -r $(Build.ArtifactStagingDirectory)/build/* $(vmUsername)@$(vmHost):/var/www/html/
      ssh -o StrictHostKeyChecking=no -i ~/.ssh/id_rsa $(vmUsername)@$(vmHost) 'sudo systemctl restart nginx'
  env:
    SSH_PRIVATE_KEY: $(vmSSHKey)
```

---

###  Output

* React application is automatically built
* Static files are securely copied to Azure VM
* App is served via Nginx or configured HTTP server

---

###  Authors

**Vaishnavi Borase**
B.Tech CSE (Data Science), R. C. Patel Institute of Technology
CSI ID: **CT\_CSI\_DV\_4845**
Email: **[221106014@rcpit.ac.in](mailto:221106014@rcpit.ac.in)**
