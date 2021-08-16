---
name: Bug report
about: Create a report to help us improve
title: ''
labels: ''
assignees: ''

---

**Describe the context**
- **Extension:** *choose between BuildQualityChecks, CreateWorkItem, and PostBuildCleanup*
- **Environment:** *are you using Azure DevOps Services (cloud) or Azure DevOps/Team Foundation Server (on-prem)?*
  - **Server version:** *if you are running on-prem, specify the version of your Azure DevOps or Team Foundation Server*
- **Agent type:** *are you running on a Microsoft-hosted or self-hosted agent?*
  - **Agent version:** *if you are running a self-hosted agent, specify its version*
- **Pipeline type:** *are you using the task in a classic build, class release, or yaml pipeline?*

**Describe the problem and expected behavior**
*Tell us what you wanted to achieve and what unexpected behavior actually happened.
Add any concrete error messages you see in your pipeline.
Add any additional information or screenshots that describe your problem in more detail.*

**Task logs**
Run your pipeline with the following variables:
- For *BuildQualityChecks*: `System.Debug` and `BQC.LogRawData` set to `true`
- For *CreateWorkItem*: `System.Debug` set to `true`
- For *PostBuildCleanup*: `System.Debug` and `PBC.LogRawData` set to `true`

Send the task log to PSGerExtSupport@microsoft.com and reference your GitHub issue.

**Attention:** The log file may contain sensitive data (e.g., server or organization names, project names, variable information). Please do not attach the log to your GitHub issue and or remove the information from the log file before attaching or sending.
