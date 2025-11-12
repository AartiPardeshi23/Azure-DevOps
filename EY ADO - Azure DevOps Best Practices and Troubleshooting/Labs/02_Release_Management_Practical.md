Excellent ✅
Here’s your **full practical, step-by-step markdown file** — ready to copy-paste and execute easily even if you’re new.

Save it as:
📄 **`02_Release_Management_Practical.md`**

---

````markdown
# 🚀 Azure DevOps — Release Management Best Practices (Hands-On Lab)

## 🎯 Objective

You’ll learn to:
1. Create a CI/CD release pipeline using YAML  
2. Build, test, and deploy your app automatically  
3. Configure approvals, environments, and variables  
4. Follow best practices for secure and reliable releases  

We’ll use **Azure DevOps Pipelines + Azure Web App** as the deployment target.

---

## 🧰 Prerequisites

✅ Azure DevOps account  
✅ A project (e.g., `EY-ADO-Project`)  
✅ A repo with sample code (Node.js, Python, or .NET — choose one)  
✅ Azure subscription (Free Tier is fine)  
✅ Git installed on your PC

---

## 🧩 Folder Setup (Sample Node.js App)

If you don’t have an app, create a small demo:

```bash
mkdir ado-release-demo
cd ado-release-demo
npm init -y
echo "console.log('Hello from Azure DevOps Release Demo!')" > app.js
````

Create a simple test:

```bash
echo "console.log('Test Passed!')" > test.js
```

Add `.gitignore`:

```
node_modules
dist
.env
```

Commit and push to Azure DevOps repo:

```bash
git init
git add .
git commit -m "Initial app commit"
git branch -M main
git remote add origin https://dev.azure.com/ORG/PROJECT/_git/ado-release-demo
git push -u origin main
```

---

## 🧱 STEP 1: Create the CI (Build) Pipeline

This pipeline will:

* Run when you push to main or develop
* Install dependencies
* Run tests
* Publish build artifacts

Create file:
📄 `.azure-pipelines/ci.yml`

```yaml
trigger:
  branches:
    include:
      - main
      - develop

pool:
  vmImage: 'ubuntu-latest'

steps:
- task: NodeTool@0
  inputs:
    versionSpec: '18.x'
  displayName: 'Install Node.js'

- script: |
    npm install
    node test.js
  displayName: 'Install dependencies & run test'

- task: ArchiveFiles@2
  inputs:
    rootFolderOrFile: '$(System.DefaultWorkingDirectory)'
    includeRootFolder: false
    archiveType: 'zip'
    archiveFile: '$(Build.ArtifactStagingDirectory)/app.zip'
    replaceExistingArchive: true

- task: PublishBuildArtifacts@1
  inputs:
    PathtoPublish: '$(Build.ArtifactStagingDirectory)'
    ArtifactName: 'drop'
    publishLocation: 'Container'
```

---

## ▶️ STEP 2: Run CI Pipeline

In Azure DevOps:

1. Go to **Pipelines → New Pipeline**
2. Choose **Azure Repos Git**
3. Select your repo → choose YAML
4. Select path `.azure-pipelines/ci.yml`
5. Click **Run**

✅ This will:

* Install Node
* Run tests
* Create and publish `app.zip` artifact

---

## 🧱 STEP 3: Create the CD (Release) Pipeline

Now we’ll deploy automatically to **Azure Web App**.

Create file:
📄 `.azure-pipelines/cd.yml`

```yaml
trigger: none

resources:
  pipelines:
    - pipeline: ciPipeline
      source: EY-ADO-CI   # Replace with your CI pipeline name
      trigger: true

pool:
  vmImage: 'ubuntu-latest'

variables:
  azureSubscription: 'MyAzureServiceConnection'
  webAppName: 'ado-demo-webapp'   # your app name in Azure
  packagePath: '$(Pipeline.Workspace)/drop/app.zip'

stages:
- stage: DeployToDev
  displayName: 'Deploy to Dev Environment'
  jobs:
  - job: Deploy
    steps:
    - download: ciPipeline
      artifact: drop

    - task: AzureWebApp@1
      inputs:
        azureSubscription: $(azureSubscription)
        appType: webAppLinux
        appName: $(webAppName)
        package: $(packagePath)
```

---

## 🔐 STEP 4: Create Azure Service Connection

1. Go to **Project Settings → Service connections → New service connection**
2. Choose **Azure Resource Manager**
3. Select “Service principal (automatic)”
4. Choose your subscription and resource group
5. Name it `MyAzureServiceConnection`

✅ This lets Azure DevOps deploy to your Azure account securely.

---

## 🏗️ STEP 5: Create an Azure Web App (Deployment Target)

Open Azure Portal → Cloud Shell or local terminal:

```bash
az login
az group create --name ado-rg --location eastus
az appservice plan create --name ado-plan --resource-group ado-rg --sku F1 --is-linux
az webapp create --resource-group ado-rg --plan ado-plan --name ado-demo-webapp --runtime "NODE|18-lts"
```

✅ Your app is live at:

```
https://ado-demo-webapp.azurewebsites.net
```

---

## ⚙️ STEP 6: Add Environments & Approvals

In Azure DevOps:

1. Go to **Pipelines → Environments**
2. Click **New Environment → Name: Dev**
3. Add optional approval (your name or manager)
4. Add another one called **Production**
5. Add **mandatory approval** before Prod deploy

This adds a human check before live deployment.

---

## 🧩 STEP 7: Run the CD Pipeline

1. Go to **Pipelines → New Pipeline**
2. Choose **YAML**
3. Select `.azure-pipelines/cd.yml`
4. Click **Run**

Azure DevOps will:

* Download build artifact from CI
* Deploy to Azure Web App
* Mark success/failure in logs

✅ Output:

```
Successfully deployed web app ado-demo-webapp
```

Visit your app:
👉 [https://ado-demo-webapp.azurewebsites.net](https://ado-demo-webapp.azurewebsites.net)

---

## 🧭 STEP 8: Add Variable Group for Secrets

Go to:
**Pipelines → Library → + Variable Group**

| Name    | Value                         | Secret |
| ------- | ----------------------------- | ------ |
| DB_CONN | dbserver.database.windows.net | No     |
| DB_PASS | yourStrongPassword            | ✅ Yes  |

Reference in YAML:

```yaml
variables:
  - group: MyVariableGroup
```

✅ Keeps your sensitive data secure.

---

## 🧩 STEP 9: Add Approval Gates (Best Practice)

In Azure DevOps:

1. Go to **Environments → Production**
2. Click “+ Add check” → **Approvals**
3. Add reviewer(s)
4. Save

Now the release waits for manual approval before deploying to production.

---

## 🧰 STEP 10: Rollback Strategy (Optional but Important)

If deployment fails:

* Go to **Pipelines → Releases → Run history**
* Click previous successful run → **Redeploy**
* Or swap staging slot to production:

  ```bash
  az webapp deployment slot swap --name ado-demo-webapp --resource-group ado-rg --slot staging --target-slot production
  ```

---

## 🧩 STEP 11: Monitor Deployment Health

In Azure Portal:

1. Go to **App Service → Logs**
2. Enable **Application Insights**
3. Review metrics like response time, failures, etc.

✅ Helps detect if release caused performance issues.

---

## 📊 STEP 12: Folder Structure Example

```
/.azure-pipelines/
  ├── ci.yml
  ├── cd.yml
/environments/
  ├── dev/
  ├── prod/
/src/
  └── app.js
```

---

## 🧠 Quick Quiz

1. What is the role of the CI pipeline?
2. Why use a service connection?
3. What does the artifact contain?
4. Why add approvals before production?
5. What command deploys to Azure Web App?

---

## ✅ Expected Outcome

| Step        | Result                       |
| ----------- | ---------------------------- |
| CI pipeline | Builds + publishes artifacts |
| CD pipeline | Deploys to Azure Web App     |
| Approvals   | Required for Production      |
| Secrets     | Stored in variable group     |
| Rollback    | Redeploy old version         |
| Monitoring  | App Insights enabled         |

---

## 🎉 Congratulations!

You have successfully:

* Built and tested code using CI
* Automated deployment using CD
* Used approvals, variable groups, and service connections
* Followed best release management practices in Azure DevOps

✅ **You now have a full CI/CD release workflow working end-to-end!**

---

```

---

Would you like me to now generate the next topic’s theory file:  
📘 **`03_Performance_Optimization_in_Azure_DevOps.md` (theory only)?**
```
