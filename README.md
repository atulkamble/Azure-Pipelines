<div align="center">
<h1>🚀 MyApp</h1>
<p><strong>Built with ❤️ by <a href="https://github.com/atulkamble">Atul Kamble</a></strong></p>

<p>
<a href="https://codespaces.new/atulkamble/template.git">
<img src="https://github.com/codespaces/badge.svg" alt="Open in GitHub Codespaces" />
</a>
<a href="https://vscode.dev/github/atulkamble/template">
<img src="https://img.shields.io/badge/Open%20with-VS%20Code-007ACC?logo=visualstudiocode&style=for-the-badge" alt="Open with VS Code" />
</a>
<a href="https://vscode.dev/redirect?url=vscode://ms-vscode-remote.remote-containers/cloneInVolume?url=https://github.com/atulkamble/template">
<img src="https://img.shields.io/badge/Dev%20Containers-Ready-blue?logo=docker&style=for-the-badge" />
</a>
<a href="https://desktop.github.com/">
<img src="https://img.shields.io/badge/GitHub-Desktop-6f42c1?logo=github&style=for-the-badge" />
</a>
</p>

<p>
<a href="https://github.com/atulkamble">
<img src="https://img.shields.io/badge/GitHub-atulkamble-181717?logo=github&style=flat-square" />
</a>
<a href="https://www.linkedin.com/in/atuljkamble/">
<img src="https://img.shields.io/badge/LinkedIn-atuljkamble-0A66C2?logo=linkedin&style=flat-square" />
</a>
<a href="https://x.com/atul_kamble">
<img src="https://img.shields.io/badge/X-@atul_kamble-000000?logo=x&style=flat-square" />
</a>
</p>

<strong>Version 1.0.0</strong> | <strong>Last Updated:</strong> January 2026
</div>

Below is a **clean, exam-ready + real-world** collection of **Azure Pipelines commands, scripts, and YAML codes** — organized into **Basic → Medium → Advanced** levels.

This is aligned for **AZ-400**, interviews, and **production CI/CD** usage in **Azure DevOps Pipelines**.

---

# 🚀 Azure Pipelines – Commands, Scripts & YAML

---

## 🔹 BASIC LEVEL (Foundations)

### 1️⃣ Basic Azure Pipeline YAML (Hello World)

```yaml
trigger:
- main

pool:
  vmImage: ubuntu-latest

steps:
- script: echo "Hello Azure Pipelines!"
  displayName: 'Hello World'
```

---

### 2️⃣ Use Predefined Variables

```yaml
steps:
- script: |
    echo "Build ID: $(Build.BuildId)"
    echo "Repo: $(Build.Repository.Name)"
    echo "Branch: $(Build.SourceBranchName)"
```

---

### 3️⃣ Install Tools (Linux Agent)

```yaml
steps:
- script: |
    sudo apt-get update
    sudo apt-get install -y git curl
```

---

### 4️⃣ Run Shell Script

```yaml
steps:
- bash: |
    echo "Running shell script"
    ls -l
```

---

### 5️⃣ Publish Build Artifact

```yaml
steps:
- publish: $(Build.ArtifactStagingDirectory)
  artifact: drop
```

---

## 🔹 MEDIUM LEVEL (CI/CD Core Skills)

---

### 6️⃣ Multi-Stage Pipeline (Build → Deploy)

```yaml
stages:
- stage: Build
  jobs:
  - job: BuildJob
    steps:
    - script: echo "Building application"

- stage: Deploy
  dependsOn: Build
  jobs:
  - job: DeployJob
    steps:
    - script: echo "Deploying application"
```

---

### 7️⃣ Variables & Variable Groups

```yaml
variables:
  appName: myapp

steps:
- script: echo "App Name: $(appName)"
```

Using Variable Group:

```yaml
variables:
- group: prod-variables
```

---

### 8️⃣ Build Docker Image

```yaml
steps:
- task: Docker@2
  inputs:
    command: build
    Dockerfile: '**/Dockerfile'
    tags: |
      $(Build.BuildId)
```

---

### 9️⃣ Conditional Execution

```yaml
steps:
- script: echo "Only on main branch"
  condition: eq(variables['Build.SourceBranch'], 'refs/heads/main')
```

---

### 🔟 Run Unit Tests (Example – Python)

```yaml
steps:
- script: |
    pip install pytest
    pytest
```

---

## 🔹 ADVANCED LEVEL (Production & Enterprise)

---

### 1️⃣ YAML Template Usage

**template.yml**

```yaml
parameters:
  env: ''

steps:
- script: echo "Deploying to ${{ parameters.env }}"
```

**azure-pipelines.yml**

```yaml
steps:
- template: template.yml
  parameters:
    env: production
```

---

### 2️⃣ Secure Secrets (Azure Key Vault)

```yaml
- task: AzureKeyVault@2
  inputs:
    azureSubscription: 'Azure-Service-Connection'
    KeyVaultName: 'prod-keyvault'
    SecretsFilter: '*'
```

---

### 3️⃣ Deploy to AKS (kubectl)

```yaml
- task: Kubernetes@1
  inputs:
    connectionType: Azure Resource Manager
    azureSubscriptionEndpoint: aks-connection
    azureResourceGroup: myRG
    kubernetesCluster: myAKS
    command: apply
    arguments: -f deployment.yaml
```

---

### 4️⃣ Environment with Approval Gates

```yaml
- deployment: DeployProd
  environment: production
  strategy:
    runOnce:
      deploy:
        steps:
        - script: echo "Deploying to PROD"
```

(Approvals configured in UI)

---

### 5️⃣ Matrix Strategy (Multiple OS)

```yaml
strategy:
  matrix:
    linux:
      vmImage: ubuntu-latest
    windows:
      vmImage: windows-latest

pool:
  vmImage: $(vmImage)

steps:
- script: echo "Running on $(vmImage)"
```

---

### 6️⃣ Manual Trigger Pipeline

```yaml
trigger: none
```

Run manually from UI.

---

### 7️⃣ Rollback Strategy Example

```yaml
- script: |
    kubectl rollout undo deployment myapp
```

---

### 8️⃣ Use Service Connection Securely

```yaml
- task: AzureCLI@2
  inputs:
    azureSubscription: 'Azure-Service-Connection'
    scriptType: bash
    scriptLocation: inlineScript
    inlineScript: |
      az group list
```

---

# 🚀 Azure Pipelines – EXTENDED YAML SYNTAX GUIDE

*(Basic → Medium → Advanced → Expert)*

---

## 🔹 BASIC SYNTAX (Core Building Blocks)

### 1️⃣ trigger / pr syntax

```yaml
trigger:
  branches:
    include:
    - main
    - develop
    exclude:
    - feature/*
```

```yaml
pr:
  branches:
    include:
    - main
```

---

### 2️⃣ pool syntaxes

```yaml
pool:
  vmImage: ubuntu-latest
```

```yaml
pool:
  name: MySelfHostedPool
```

---

### 3️⃣ steps syntaxes

```yaml
steps:
- script: echo "hello"
```

```yaml
steps:
- bash: echo "hello"
```

```yaml
steps:
- pwsh: Write-Host "hello"
```

---

### 4️⃣ displayName syntax

```yaml
- script: echo "Build"
  displayName: 'Build Step'
```

---

### 5️⃣ workingDirectory

```yaml
- script: mvn clean package
  workingDirectory: app/
```

---

## 🔹 VARIABLES – FULL SYNTAX COVERAGE

### 6️⃣ Inline variables

```yaml
variables:
  name: atul
  env: dev
```

---

### 7️⃣ Runtime vs Compile-time variables

```yaml
$(variableName)        # runtime
${{ variables.name }} # compile-time
```

---

### 8️⃣ Variable Groups

```yaml
variables:
- group: prod-secrets
```

---

### 9️⃣ Output variables (IMPORTANT)

```yaml
- bash: |
    echo "##vso[task.setvariable variable=VERSION;isOutput=true]1.0.0"
  name: setVar
```

Usage:

```yaml
$(setVar.VERSION)
```

---

### 🔟 Secret masking syntax

```yaml
variables:
- name: password
  value: $(MY_SECRET)
```

(secrets masked automatically)

---

## 🔹 JOBS & STAGES – DEEP SYNTAX

---

### 1️⃣ Job syntax variations

```yaml
jobs:
- job: Build
  steps:
  - script: echo "build"
```

```yaml
jobs:
- job: Test
  dependsOn: Build
```

---

### 2️⃣ Timeout & cancel timeout

```yaml
jobs:
- job: LongJob
  timeoutInMinutes: 30
  cancelTimeoutInMinutes: 5
```

---

### 3️⃣ Continue on error

```yaml
- script: exit 1
  continueOnError: true
```

---

### 4️⃣ Stage conditions

```yaml
condition: succeeded()
condition: failed()
condition: always()
```

```yaml
condition: and(succeeded(), eq(variables.env, 'prod'))
```

---

## 🔹 CONDITIONAL & EXPRESSIONS (VERY IMPORTANT)

---

### 1️⃣ if / else syntax

```yaml
- ${{ if eq(variables.env, 'prod') }}:
  - script: echo "Production"
```

---

### 2️⃣ Branch-based condition

```yaml
condition: startsWith(variables['Build.SourceBranch'], 'refs/heads/release')
```

---

### 3️⃣ Boolean expression syntax

```yaml
condition: or(eq(variables.env,'dev'), eq(variables.env,'test'))
```

---

## 🔹 TASK SYNTAX (Most Used)

---

### Azure CLI

```yaml
- task: AzureCLI@2
  inputs:
    azureSubscription: MyConnection
    scriptType: bash
    scriptLocation: inlineScript
    inlineScript: az vm list
```

---

### Docker Login / Push

```yaml
- task: Docker@2
  inputs:
    command: login
    containerRegistry: ACRConnection
```

```yaml
- task: Docker@2
  inputs:
    command: push
    tags: latest
```

---

### Copy Files

```yaml
- task: CopyFiles@2
  inputs:
    sourceFolder: src
    contents: '**'
    targetFolder: $(Build.ArtifactStagingDirectory)
```

---

### Publish / Download Artifacts

```yaml
- task: PublishBuildArtifacts@1
```

```yaml
- task: DownloadPipelineArtifact@2
```

---

## 🔹 DEPLOYMENT & ENVIRONMENT SYNTAX

---

### 1️⃣ Deployment job

```yaml
- deployment: DeployApp
  environment: prod
  strategy:
    runOnce:
      deploy:
        steps:
        - script: echo "Deploying"
```

---

### 2️⃣ Environment approvals (YAML reference)

```yaml
environment:
  name: production
  resourceType: Kubernetes
```

(Approvals configured via UI)

---

## 🔹 TEMPLATE & REUSABILITY (ADVANCED)

---

### Template with parameters

```yaml
parameters:
- name: env
  type: string
```

Usage:

```yaml
- template: deploy.yml
  parameters:
    env: prod
```

---

### Extend template

```yaml
extends:
  template: base.yml
```

---

## 🔹 MATRIX & PARALLELISM

---

### Matrix strategy

```yaml
strategy:
  matrix:
    node16:
      NODE_VERSION: 16
    node18:
      NODE_VERSION: 18
```

---

### Parallel jobs

```yaml
strategy:
  parallel: 3
```

---

## 🔹 TRIGGERS – ADVANCED

---

### Path-based trigger

```yaml
trigger:
  paths:
    include:
    - src/**
```

---

### Schedule trigger (CRON)

```yaml
schedules:
- cron: "0 2 * * *"
  branches:
    include:
    - main
```

---

## 🔹 AKS & KUBERNETES SYNTAX

---

### kubectl via script

```yaml
- script: |
    kubectl apply -f deployment.yaml
```

---

### Kubernetes task

```yaml
- task: Kubernetes@1
  inputs:
    command: get
    arguments: pods
```

---

## 🔹 SECURITY & GOVERNANCE

---

### Secure files

```yaml
- task: DownloadSecureFile@1
  name: kubeconfig
```

---

### Key Vault secrets

```yaml
- task: AzureKeyVault@2
```

---

## 🔹 DEBUGGING & LOGGING

---

### Enable debug logs

```yaml
variables:
  system.debug: true
```

---

### Custom log command

```bash
echo "##vso[task.logissue type=warning]This is warning"
```
---

## 🧠 Must-Know Azure Pipelines Concepts (Exam + Interviews)

✅ Agent vs Agent Pool
✅ Microsoft-hosted vs Self-hosted agent
✅ Stages → Jobs → Steps
✅ Pipeline Artifacts vs Build Artifacts
✅ Variable Groups & Secrets
✅ Service Connections
✅ Environments & Approvals
✅ YAML Templates
✅ CI vs CD Triggers

## 🔹 MUST-REMEMBER EXAM & INTERVIEW POINTS

✔ `$( )` vs `${{ }}`
✔ Runtime vs compile-time evaluation
✔ Deployment job vs normal job
✔ Variable groups vs Key Vault
✔ Hosted vs Self-Hosted agents
✔ Templates vs Extends
✔ Conditions vs Expressions
✔ Environments & approvals

---

