# Azure DevOps Release Pipelines: Applying Pre and Post-Deployment Approvers

## Overview

This documentation explains how to configure **pre-deployment** and **post-deployment approval gates** in Azure DevOps Release Pipelines. Approval gates ensure that key stakeholders validate and authorize deployments, enforcing control and compliance in your CI/CD process.

## Objectives

- Understand the role and benefits of deployment approvals.
- Configure **pre-deployment approvers** to validate before deployment starts.
- Configure **post-deployment approvers** to review after deployment completes.
- Automate notifications and track approval status.
- Apply approvals using the Azure DevOps classic release pipeline UI and YAML pipelines.

## Prerequisites

- Azure DevOps Organization and Project with Release Pipelines enabled.
- Appropriate permissions to edit release pipelines and manage approvals.
- Defined release pipeline with at least one stage/environment.

## Concepts

- **Pre-deployment Approvers:** Individuals or groups who must approve before a deployment starts to an environment.
- **Post-deployment Approvers:** Individuals or groups who review and approve after a deployment to an environment finishes.
- **Automatic and Manual Approvals:** Approvals can be manual or automated via checks.

## Steps to Configure Approvals

### Using Classic Release Pipelines UI

1. Navigate to your release pipeline in Azure DevOps.
2. Click on the **Environments** (e.g., Dev, Test, Production).
3. Click the **Pre-deployment conditions** (lightning bolt icon).
4. Under **Approvals**, add users or groups as **Pre-deployment approvers**.
5. Optionally configure timeout, timeout behavior, and approval policies.
6. Similarly, click **Post-deployment conditions** and add **Post-deployment approvers**.
7. Save and create a release to test approval gates.

### Using YAML Pipelines (Environments)

In YAML pipelines, approvals are configured on **Environments** in Azure DevOps.

1. Navigate to **Pipelines > Environments**.
2. Create or select an environment.
3. Under **Approvals and Checks**, add one or more **Approvals**.
4. Specify users or groups and optional timeout.

Example YAML referencing environment with approval:

```yaml
stages:
- stage: Deploy
  jobs:
  - deployment: DeployJob
    environment: 'Production'  # Environment with approval configured
    strategy:
      runOnce:
        deploy:
          steps:
          - script: echo "Deploying to Production"
```

When a deployment targets the environment, the approval process triggers automatically.

## Best Practices

- Use environment-specific approvals to control deployment flow.
- Assign multiple approvers with majority or all required policies.
- Configure timeout policies to handle stale approvals.
- Notify approvers via email or Teams to reduce delays.
- Track approval history for audit and compliance.

## Resources

- [Azure DevOps Approvals and Checks Documentation](https://learn.microsoft.com/en-us/azure/devops/pipelines/release/approvals/?view=azure-devops&tabs=yaml)
- [Azure DevOps Release Pipelines Video Tutorial](https://www.youtube.com/watch?v=Cu7zx9u1sOE)

## Author

**Vaishnavi Boras**  
B.Tech CSE (Data Science), R. C. Patel Institute of Technology  
CSI ID: **CT_CSI_DV_4845**  
Email: **221106014@rcpit.ac.in**


