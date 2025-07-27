
#  Azure DevOps Pipelines: Using Pipeline Variables

This guide provides a professional overview on how to use **Pipeline Variables** in Azure DevOps, enabling dynamic and flexible pipeline configurations. Variables allow you to store and reuse values throughout your pipeline YAML files and scripts.

---

## What Are Pipeline Variables?

Pipeline variables are key-value pairs used in Azure DevOps pipelines to store data and control execution logic dynamically. They can be defined at various levels: pipeline-level, stage, job, or step.

---

##  Types of Variables

### 1. **User-Defined Variables**
Defined explicitly in the pipeline YAML or via the Azure DevOps UI.
```yaml
variables:
  environment: 'production'
  buildConfiguration: 'Release'
```

### 2. **System Variables**
Predefined by Azure DevOps. Examples:
- `Build.BuildId`
- `Build.SourceBranch`
- `Agent.OS`

### 3. **Environment Variables**
Accessible in scripts:
```bash
echo "Environment: $ENVIRONMENT"
```

### 4. **Output Variables**
Defined in one job or step and used in another.
```yaml
jobs:
- job: JobA
  steps:
    - script: echo "##vso[task.setvariable variable=version]1.0.0"

- job: JobB
  dependsOn: JobA
  steps:
    - script: echo "Using version $(version)"
```

---

##  Defining and Using Variables in YAML

```yaml
trigger:
- main

variables:
  appName: 'MyApp'
  imageTag: '$(Build.BuildId)'

stages:
- stage: Build
  jobs:
  - job: BuildJob
    steps:
    - script: echo "Building $(appName) with tag $(imageTag)"
```

---

##  Secrets and Variable Groups

- Use **Variable Groups** to store and manage variables across multiple pipelines.
- Store sensitive data securely using **Secret Variables** (hidden from logs).
- Link to Azure Key Vault for advanced secret management.

---

##  Runtime Expressions

You can evaluate variables using expressions:
```yaml
${{ if eq(variables['Build.SourceBranch'], 'refs/heads/main') }}:
  condition: true
```

---

##  Best Practices

- Use `$(variable)` syntax in scripts or YAML.
- Keep secrets out of logs with secret masking.
- Use variable groups to centralize and reuse values.
- Document variables in your pipeline for maintainability.
- Prefix custom variables to avoid conflict with system variables.

---

##  Resources

-  [Azure DevOps Pipeline Variables (Microsoft Docs)](https://learn.microsoft.com/en-us/azure/devops/pipelines/process/variables?view=azure-devops&tabs=yaml%2Cbatch)
-  [YouTube Tutorial: Azure Pipelines with Variables](https://www.youtube.com/watch?v=xH5EY7FCFQw)

---

##  Author

**Vaishnavi Borase**  
CSI ID: `CT_CSI_DV_4845`  
Azure Account: `221106014.@rcpit.ac.in`

---
