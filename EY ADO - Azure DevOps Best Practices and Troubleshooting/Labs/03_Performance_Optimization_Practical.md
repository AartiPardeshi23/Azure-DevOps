Awesome 💪
Here’s your **ready-to-copy practical markdown file** for Azure DevOps **Performance Optimization** —
with easy, step-by-step instructions for beginners.

Save it as:
📄 **`03_Performance_Optimization_Practical.md`**

---

````markdown
# ⚡ Azure DevOps — Performance Optimization (Hands-On Lab)

## 🎯 Objective

You will:
1. Optimize your Azure DevOps pipelines for faster build times  
2. Implement caching, shallow cloning, and artifact cleanup  
3. Compare performance before and after optimization  
4. Learn real-world tips to make CI/CD efficient and cost-effective  

---

## 🧰 Prerequisites

✅ Azure DevOps project with a sample repo (Node.js / Python / .NET)  
✅ Git installed on your computer  
✅ Basic working CI pipeline (you can reuse one from previous exercises)  

---

## ⚙️ STEP 1: Create a Sample Repository

If you don’t already have one:

```bash
mkdir ado-performance-demo
cd ado-performance-demo
npm init -y
echo "console.log('Performance Optimization Demo')" > app.js
````

Create a simple test:

```bash
echo "console.log('Test Passed')" > test.js
```

Add `.gitignore`:

```
node_modules
dist
.env
```

Push to Azure DevOps repo:

```bash
git init
git add .
git commit -m "Initial commit"
git branch -M main
git remote add origin https://dev.azure.com/ORG/PROJECT/_git/ado-performance-demo
git push -u origin main
```

---

## ⏱️ STEP 2: Create a “Non-Optimized” Pipeline (for comparison)

Create file:
📄 `.azure-pipelines/ci-non-optimized.yml`

```yaml
trigger:
  branches:
    include:
      - main

pool:
  vmImage: 'ubuntu-latest'

steps:
- script: |
    echo "Installing dependencies..."
    npm install
  displayName: 'Install Dependencies'

- script: |
    echo "Running tests..."
    node test.js
  displayName: 'Run Tests'

- script: |
    echo "Building app..."
    mkdir dist
    cp app.js dist/
  displayName: 'Build App'

- task: PublishBuildArtifacts@1
  inputs:
    PathtoPublish: 'dist'
    ArtifactName: 'drop'
```

✅ This version does *not* use caching, shallow clone, or optimization.

---

## ▶️ STEP 3: Run Non-Optimized Pipeline

1. Push `.azure-pipelines/ci-non-optimized.yml`
2. In Azure DevOps → **Pipelines → New pipeline**
3. Select YAML → choose this file → **Run**

⏱️ Note the total time (e.g., 3–5 minutes).

We’ll improve that next.

---

## ⚡ STEP 4: Create an Optimized Pipeline

Create file:
📄 `.azure-pipelines/ci-optimized.yml`

```yaml
trigger:
  branches:
    include:
      - main
      - develop

pr:
  branches:
    include:
      - feature/*

pool:
  vmImage: 'ubuntu-latest'

variables:
  buildConfiguration: 'Release'

steps:
# 🧩 Shallow clone - fetch only latest commit
- checkout: self
  fetchDepth: 1

# 💾 Cache npm dependencies
- task: Cache@2
  inputs:
    key: 'npm | "$(Agent.OS)" | package-lock.json'
    path: ~/.npm
  displayName: 'Cache npm dependencies'

# 📦 Install dependencies
- script: |
    npm ci
  displayName: 'Install dependencies (using cache)'

# 🧪 Run tests
- script: |
    node test.js
  displayName: 'Run Unit Tests'

# ⚙️ Build app
- script: |
    mkdir dist
    cp app.js dist/
  displayName: 'Build Application'

# 📚 Archive and compress output
- task: ArchiveFiles@2
  inputs:
    rootFolderOrFile: 'dist'
    includeRootFolder: false
    archiveType: 'zip'
    archiveFile: '$(Build.ArtifactStagingDirectory)/app.zip'
    replaceExistingArchive: true

# 🚀 Publish artifacts
- task: PublishBuildArtifacts@1
  inputs:
    PathtoPublish: '$(Build.ArtifactStagingDirectory)'
    ArtifactName: 'drop'
    publishLocation: 'Container'
```

✅ This optimized pipeline includes:

* Shallow clone (`fetchDepth: 1`)
* Dependency caching (`Cache@2`)
* Compressed artifact output
* Conditional triggers

---

## ▶️ STEP 5: Run Optimized Pipeline

1. Push `.azure-pipelines/ci-optimized.yml`
2. In Azure DevOps → **Pipelines → New Pipeline**
3. Choose YAML → select `.azure-pipelines/ci-optimized.yml`
4. Click **Run**

Observe:

* Faster dependency install (cache hit after 1st run)
* Faster checkout
* Smaller artifact upload size

---

## 📊 STEP 6: Compare Build Times

| Stage                | Non-Optimized | Optimized      |
| -------------------- | ------------- | -------------- |
| Clone Repo           | ~30s          | ~10s           |
| Install Dependencies | ~60–90s       | ~15–20s        |
| Build & Test         | ~60s          | ~45s           |
| Publish Artifacts    | ~40s          | ~15s           |
| **Total Time**       | **~4–5 min**  | **~1.5–2 min** |

✅ 50–60% faster build — **without any code change.**

---

## 🧹 STEP 7: Add Retention Policy (Clean Old Builds)

1. Go to **Project Settings → Retention Policies**
2. Set:

   * Keep 10 successful runs
   * Keep only last 3 artifacts
3. Save

**Why:** Keeps storage clean and faster.

---

## 🧩 STEP 8: Use Parallel Jobs (Optional Bonus)

If you have multiple test sets, run them in parallel.

Example:

```yaml
strategy:
  matrix:
    node16:
      node_version: '16.x'
    node18:
      node_version: '18.x'

steps:
- task: NodeTool@0
  inputs:
    versionSpec: '$(node_version)'
  displayName: 'Use Node $(node_version)'

- script: |
    npm ci
    node test.js
  displayName: 'Run Tests on Node $(node_version)'
```

✅ Both Node versions will run simultaneously, cutting test time in half.

---

## ⚙️ STEP 9: Use Self-Hosted Agent (Optional)

If you run pipelines often, use a **self-hosted agent** to avoid queue delays.

### On your local VM or server:

```bash
# 1. Download agent
mkdir myagent && cd myagent
curl -O https://vstsagentpackage.azureedge.net/agent/3.236.1/vsts-agent-linux-x64-3.236.1.tar.gz
tar zxvf vsts-agent-linux-x64-3.236.1.tar.gz

# 2. Configure agent
./config.sh
# Enter your Azure DevOps URL and PAT (personal access token)

# 3. Run agent
./run.sh
```

Now assign this agent pool to your pipeline:

```yaml
pool:
  name: 'SelfHostedPool'
```

✅ Self-hosted agents reuse environment and caches → even faster builds.

---

## 🔍 STEP 10: Enable Diagnostic Logs (For Debugging)

In pipeline run page →
**View Logs → Enable Diagnostic Logs**

Use these logs to:

* Identify long-running steps
* Find bottlenecks
* Tune specific tasks

---

## 🧠 STEP 11: Verification Checklist

| Optimization                    | Implemented | Verified |
| ------------------------------- | ----------- | -------- |
| Shallow clone (`fetchDepth: 1`) | ✅           | ✅        |
| Cache dependencies              | ✅           | ✅        |
| Compressed artifacts            | ✅           | ✅        |
| Path triggers                   | ✅           | ✅        |
| Retention policy                | ✅           | ✅        |
| Parallel jobs                   | ✅           | Optional |
| Self-hosted agent               | ✅           | Optional |

---

## 💡 STEP 12: Tips for Continuous Improvement

1. Measure build time after every change
2. Use dashboards → Pipelines → Analytics → Duration Trend
3. Clean unused variables & templates
4. Use smaller base images for containerized builds
5. Review YAML for redundant tasks every month

---

## 🧩 STEP 13: Troubleshooting Common Issues

| Issue                    | Cause             | Fix                         |
| ------------------------ | ----------------- | --------------------------- |
| Cache not restoring      | Key mismatch      | Keep same key format        |
| Slow clone               | Large repo        | Use `fetchDepth: 1`         |
| Pipeline queues too long | Shared agent pool | Add self-hosted agent       |
| Artifact upload fails    | Too large         | Compress or filter files    |
| Logs too large           | Verbose output    | Use `quiet` flag in scripts |

---

## 📘 Step-by-Step Summary

| Step | Action                        | Result              |
| ---- | ----------------------------- | ------------------- |
| 1    | Setup repo                    | Working sample      |
| 2    | Create non-optimized pipeline | Baseline build time |
| 3    | Create optimized pipeline     | Faster build        |
| 4    | Add caching                   | Reuse dependencies  |
| 5    | Add shallow clone             | Reduce fetch time   |
| 6    | Add retention policy          | Clean storage       |
| 7    | Add parallel jobs             | Speed up tests      |
| 8    | (Optional) Self-hosted agent  | Remove queue delays |

---

## 🧠 Quick Quiz

1. What does caching do in pipelines?
2. Why use `fetchDepth: 1`?
3. How can you reduce storage usage?
4. What are the benefits of a self-hosted agent?
5. What’s the easiest way to run parallel builds?

---

## ✅ Expected Outcome

* Build time reduced by 50–60%
* Faster feedback to developers
* Lower Azure DevOps pipeline costs
* Smaller artifacts and cleaner history

🎉 **You’ve successfully optimized performance in Azure DevOps pipelines!**

---

```

---

Would you like me to now generate the **next theory file**:  
📘 `04_Troubleshooting_Common_Issues.md` (theory only)?
```
