# Azure DevOps: Creating a Linux/Windows Self-Hosted Agent

## Overview

This guide explains how to configure a **self-hosted agent** on **Linux or Windows** for Azure DevOps Pipelines. A self-hosted agent gives more control over the build environment, is customizable, and can reduce pipeline runtime costs compared to Microsoft-hosted agents.

## Objectives

- Understand the role of self-hosted agents in Azure Pipelines.
- Set up a self-hosted agent on a Linux or Windows machine.
- Register the agent with your Azure DevOps organization.
- Run a test build to verify agent registration and execution.

## Prerequisites

- A physical or virtual machine running **Windows** or **Linux** with internet access.
- Admin/root access on the machine.
- An Azure DevOps organization and project.
- Agent pool creation permissions in Azure DevOps.

## Step-by-Step Setup

### Step 1: Create an Agent Pool (Optional)

1. Go to **Project Settings > Agent Pools**.
2. Click **Add pool**.
3. Enter a name (e.g., `SelfHostedPool`) and click **Create**.

### Step 2: Download and Configure the Agent

#### For Linux:

1. SSH into your Linux machine.
2. Install required dependencies:
   ```bash
   sudo apt update
   sudo apt install -y libcurl4 openssh-client unzip
   ```

3. Download the agent package:
   ```bash
   mkdir myagent && cd myagent
   wget https://vstsagentpackage.azureedge.net/agent/3.236.1/vsts-agent-linux-x64-3.236.1.tar.gz
   tar zxvf vsts-agent-linux-x64-3.236.1.tar.gz
   ```

4. Configure the agent:
   ```bash
   ./config.sh
   ```

   Provide the following:
   - Azure DevOps URL (e.g., `https://dev.azure.com/yourorganization`)
   - Personal Access Token (PAT)
   - Agent pool name (e.g., `SelfHostedPool`)
   - Agent name
   - Work folder (default is `_work`)

5. Start the agent:
   ```bash
   ./svc.sh install
   ./svc.sh start
   ```

#### For Windows:

1. Download the agent from:  
   [https://learn.microsoft.com/en-us/azure/devops/pipelines/agents/windows-agent?view=azure-devops](https://learn.microsoft.com/en-us/azure/devops/pipelines/agents/windows-agent?view=azure-devops)

2. Extract the zip to `C:\agent`.

3. Open Command Prompt as Administrator and run:

   ```cmd
   cd C:\agent
   config.cmd
   ```

   Provide:
   - Azure DevOps URL
   - PAT
   - Agent pool name
   - Agent name
   - Work folder

4. Install and start the agent as a service:

   ```cmd
   .\svc install
   .\svc start
   ```

### Step 3: Verify Agent Status

1. Go to **Project Settings > Agent Pools > SelfHostedPool**.
2. The newly added agent should show as **online** and **idle**.
3. You can now use this agent in your pipeline YAML:

```yaml
pool:
  name: SelfHostedPool
```

## Best Practices

- Use firewalls and antivirus software to secure the agent machine.
- Install only required tools to reduce attack surface.
- Rotate PATs regularly and restrict their scope.
- Run the agent as a non-root/non-admin user when possible.
- Use tags to target specific agents for specific jobs.

## Resources

- [Create a Linux Self-Hosted Agent (Microsoft Docs)](https://learn.microsoft.com/en-us/azure/devops/pipelines/agents/linux-agent?view=azure-devops&tabs=IP-V4)
- [Create a Windows Self-Hosted Agent (Microsoft Docs)](https://learn.microsoft.com/en-us/azure/devops/pipelines/agents/windows-agent?view=azure-devops)
- [Video: Azure DevOps Test Plans & Agent Setup](https://www.youtube.com/watch?v=Cu7zx9u1sOE)

## Author

**Vaishnavi Borase**  
B.Tech CSE (Data Science), R. C. Patel Institute of Technology  
CSI ID: **CT_CSI_DV_4845**  
Email: **221106014@rcpit.ac.in**


