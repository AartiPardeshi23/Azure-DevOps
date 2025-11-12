# 🧭 EY ADO — Azure DevOps Best Practices & Troubleshooting  

---

## 🎯 Objective

This repository contains **step-by-step guides, theory, and practical labs** for mastering **Azure DevOps Best Practices and Troubleshooting**, even if you’re a beginner.

You will learn:
- 🌿 Branching and Merging Strategies  
- 🚀 Release Management Best Practices  
- ⚡ Pipeline and Performance Optimization  
- 🧯 Troubleshooting Common Issues  

---

## 🗂️ Repository Structure

```
/Theory.docx
/Labs
    /01_Branching_and_Merging_Strategies.md
    /02_Release_Management_Practical.md
    /03_Performance_Optimization_Practical.md
    /04_Troubleshooting_Common_Issues.md
README.md

```

---

## 📘 01 — Branching and Merging Strategies

### 🧠 Key Concepts
- Importance of branching in version control  
- Common branching models: `main`, `develop`, `feature/*`, `release/*`, `hotfix/*`  
- Pull Requests (PRs) for collaboration and review  
- Branch protection rules in Azure DevOps  
- Merge strategies: Merge Commit, Squash, Rebase  

### 🧪 Practical Highlights
- Created and pushed branches using Git  
- Configured branch policies in Azure DevOps  
- Created PRs with reviewers and build validation  
- Resolved merge conflicts manually  
- Protected the `main` branch from direct pushes  

📄 **Files:**  
- [`01_Branching_and_Merging_Strategies.md`](01_Branching_and_Merging_Strategies.md)

---

## 🚀 02 — Release Management Best Practices

### 🧠 Key Concepts
- CI/CD Pipelines overview (Continuous Integration & Delivery)  
- Build, Test, and Deploy stages  
- Artifacts, Environments, and Approvals  
- Service Connections for secure deployments  
- Rollback and version control of releases  

### 🧪 Practical Highlights
- Created `.azure-pipelines/ci.yml` for build & test  
- Created `.azure-pipelines/cd.yml` for deployment  
- Connected Azure DevOps to Azure using Service Connection  
- Deployed to Azure Web App automatically  
- Added manual approval gates before Production  

📄 **Files:**  
- [`02_Release_Management_Practical.md`](02_Release_Management_Practical.md)

---

## ⚡ 03 — Performance Optimization in Azure DevOps

### 🧠 Key Concepts
- Optimize pipeline performance and cost  
- Use caching, shallow clones, and retention policies  
- Use parallel jobs for faster testing  
- Self-hosted agents for speed and control  
- Artifact compression and cleanup strategies  

### 🧪 Practical Highlights
- Created `ci-non-optimized.yml` and `ci-optimized.yml`  
- Compared build time before vs after optimization  
- Implemented dependency caching (`Cache@2`)  
- Added retention policies for older builds  
- Used matrix builds and self-hosted agents  

📄 **Files:**  
- [`03_Performance_Optimization_Practical.md`](03_Performance_Optimization_Practical.md)

---

## 🧯 04 — Troubleshooting Common Issues

### 🧠 Key Concepts
- Common pipeline and repo issues  
- Authentication & permission failures  
- Merge conflicts & branch policy issues  
- Agent, artifact, and deployment problems  
- Debug logs and system diagnostics  

### 🧪 Practical Highlights
- Simulated pipeline build failure and fixed it  
- Recreated broken Azure service connections  
- Resolved merge conflicts manually  
- Fixed artifact path mismatch and timeout errors  
- Used system debug logs and diagnostic tools  

📄 **Files:**  
- [`04_Troubleshooting_Common_Issues.md`](04_Troubleshooting_Common_Issues.md)  

---

## 🧰 Tools & Services Used

| Tool | Purpose |
|------|----------|
| **Azure DevOps** | Code, Pipelines, Repos, Artifacts |
| **Git & Git Bash / VS Code** | Source Control |
| **Azure Portal** | Web App, Resource Groups |
| **Azure CLI** | Scripting & Deployment |
| **Node.js / Python** | Sample Application |
| **YAML Pipelines** | CI/CD Automation |

---

## 💡 Best Practices Summary

| Category | Best Practice | Benefit |
|-----------|----------------|----------|
| Branching | Use feature branches & PRs | Clean, traceable history |
| CI/CD | Automate builds & tests | Faster feedback |
| Security | Protect `main` branch, secure secrets | Prevent mistakes |
| Performance | Use caching, shallow clone | Faster pipelines |
| Troubleshooting | Read logs & check service connections | Quick issue resolution |

---

## 📊 Learning Flow

| Day | Topic | Output |
|-----|--------|---------|
| **Day 1** | Branching & Merging | Feature → Develop → Main workflow |
| **Day 2** | Release Management | CI/CD pipeline deployment |
| **Day 3** | Performance Optimization | Faster, efficient pipelines |
| **Day 4** | Troubleshooting | Fixing common DevOps errors |

---

## 🧠 Quiz & Review

✅ **Sample Questions to Self-Test:**
1. What are the advantages of squash merging?  
2. What’s the difference between CI and CD?  
3. How can caching improve pipeline speed?  
4. What’s the role of a service connection?  
5. How do you resolve a merge conflict in Azure DevOps?

---

## 📚 References & Resources

- [Azure DevOps Documentation](https://learn.microsoft.com/en-us/azure/devops)  
- [YAML Pipeline Syntax](https://learn.microsoft.com/en-us/azure/devops/pipelines/yaml-schema)  
- [Azure CLI Reference](https://learn.microsoft.com/en-us/cli/azure)  
- [Git Branching Guide](https://www.atlassian.com/git/tutorials/using-branches)  
- [Azure Web App Deployment](https://learn.microsoft.com/en-us/azure/app-service/deploy-azure-pipelines)  

---
