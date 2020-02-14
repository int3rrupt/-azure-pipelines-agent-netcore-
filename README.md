# Azure Pipelines Agents for Dotnet Core
This repository contains Azure Pipelines Self-Hosted Docker Agents for Dotnet Core Builds and Deployments.  
**Documentation**: https://docs.microsoft.com/en-us/azure/devops/pipelines/agents/docker

## Environment Variables
### AZP_URL
The environment variable to the organization's Azure DevOps resources.  
ex: `https://dev.azure.com/TeamCSSP`

### AZP_TOKEN
A Personal Access Token (PAT) that provides the agent with access to the organization's Azure DevOps.  
#### Minimum Required Permissions
Name | Value | Notes
-----|-------|------
Agent Pools | Read | 
Build | Read | 
Code | Read | 
Deployment Groups | Read & manage | 
Packaging | Read & write | This is only required if working with Azure Feeds such as a private NuGet feed hosted in Azure DevOps.
Release | Read | 

## Available agents
Type | Name | Notes
-----|------|------
Default | Default | This is the default agent with no additional tools. Can be used as a starting point for a new custom agent. Can also be used for basic deployments.

## Running the agent

```bash
docker run `
-e AZP_URL="https://dev.azure.com/<Organization>" `
-e AZP_TOKEN=<PAT Token> `
-e AZP_AGENT_NAME=<Agent Name> `
-e AZP_POOL=<Agent Pool Name> `
-e VSTS_ARM_REST_IGNORE_SSL_ERRORS=<True|False> `
--add-host <Hostname>:<IP> `
<Image Name>:<Tag>
```
```bash
docker run `
-e AZP_URL="https://dev.azure.com/TeamCSSP" `
-e AZP_TOKEN=s0Mep3rs0naLAcC3sT0k3n `
-e AZP_AGENT_NAME=InternalReleaseAgent `
-e AZP_POOL=InternalAgents `
-e VSTS_ARM_REST_IGNORE_SSL_ERRORS=True `
--add-host someapp.ace.appservicewebsites.net:10.0.0.0 `
--add-host someapp.scm.ace.appservicewebsites.net:10.0.0.0 `
internalreleaseagent:latest
```
