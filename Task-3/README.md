# Azure DevOps Pipelines: Using Variable Groups, Task Groups, and Scoped Variables

## Overview

This repository demonstrates the usage of **Variable Groups**, **Task Groups**, and how to **set variable scopes across different stages** in Azure DevOps Pipelines. These features help in maintaining clean, modular, and reusable CI/CD pipelines across multiple projects and environments.

## Objectives

- Create and link **Variable Groups** in Azure Pipelines.
- Define **Task Groups** to encapsulate reusable task sequences.
- Apply **stage-specific variable scoping** in YAML pipelines.
- Demonstrate best practices in pipeline design for maintainability and clarity.

## Prerequisites

- Azure DevOps Organization and Project.
- Basic understanding of YAML-based pipelines.
- Azure Pipelines access with permissions to edit pipelines and libraries.

## Repository Structure

```
azure-pipelines/
├── azure-pipelines.yml          # Main pipeline definition file
├── task-groups/
│   └── deploy-task-group.yml    # Task group template for deployment
├── docs/
│   └── usage-guide.md           # Detailed usage documentation
```

## Steps to Reproduce

### 1. Create Variable Group

1. Navigate to **Pipelines > Library** in Azure DevOps.
2. Click **+ Variable group** and define a name (e.g., `CommonVariables`).
3. Add variables such as:
   - `appName`: your-app-name
   - `environment`: dev
4. Set secrets if needed and enable "Allow access to all pipelines" or scope it to selected pipelines.

### 2. Use Variable Group in YAML Pipeline

```yaml
variables:
  - group: CommonVariables
```

### 3. Set Variable Scope for Stages

```yaml
stages:
- stage: Build
  variables:
    buildEnv: dev
  jobs:
  - job: BuildJob
    steps:
    - script: echo "Building in $(buildEnv)"

- stage: Deploy
  variables:
    deployEnv: production
  jobs:
  - job: DeployJob
    steps:
    - script: echo "Deploying to $(deployEnv)"
```

### 4. Create and Use a Task Group

1. Navigate to **Pipelines > Task Groups**.
2. Create a new task group for deployment with parameters such as:
   - `ConnectionName`
   - `ResourceGroup`
3. Add necessary deployment tasks (e.g., Azure App Service Deploy).
4. Save and use this task group in classic or YAML pipelines using the `task` reference.

#### Sample YAML Using a Task Group

```yaml
steps:
- task: <TaskGroupName>@1
  inputs:
    ConnectionName: 'MyServiceConnection'
    ResourceGroup: 'my-rg'
```

## Best Practices

- Use variable groups to centralize configuration for different environments.
- Leverage task groups to encapsulate common deployment logic.
- Always set variable scopes clearly to avoid ambiguity across stages.
- Use secrets in variable groups for sensitive data like credentials.
- Document variable usage and pipeline flow in version-controlled markdown files.

## Resources

- [Azure DevOps Variable Groups](https://learn.microsoft.com/en-us/azure/devops/pipelines/library/variable-groups?view=azure-devops&tabs=azure-pipelines-ui%2Cyaml)
- [Azure DevOps Task Groups](https://learn.microsoft.com/en-us/azure/devops/pipelines/library/task-groups?view=azure-devops)
- [Video Tutorial on Azure DevOps Plans](https://www.youtube.com/watch?v=Cu7zx9u1sOE)

## Author

**Vaishnavi Borase**  
B.Tech CSE (Data Science), R. C. Patel Institute of Technology  
CSI ID: **CT_CSI_DV_4845**  
Email: **221106014@rcpit.ac.in**

---

This repository is intended for learning and demonstration purposes and does not contain production secrets or credentials.

