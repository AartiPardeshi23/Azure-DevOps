Perfect 👏
Here’s your **ready-to-copy practical Markdown file** for:

📄 **`04_Troubleshooting_Practical.md`**

This file includes **real hands-on debugging, commands, and Azure DevOps UI steps** — written in a slow, easy-to-follow format.

---

````markdown
# 🧯 Azure DevOps — Troubleshooting Common Issues (Hands-On Lab)

## 🎯 Objective

You will learn how to:
1. Identify and fix common Azure DevOps issues.  
2. Read logs and debug failed pipelines.  
3. Solve authentication, build, and merge errors.  
4. Use Azure DevOps tools for diagnostics.

---

## 🧰 Prerequisites

✅ Azure DevOps project (with pipeline already created)  
✅ A repository with a working YAML pipeline (Node, Python, or .NET app)  
✅ Git and Azure CLI installed locally  

---

# 🧩 SECTION 1 — Pipeline Failures

---

## ⚙️ 1. Simulate a Pipeline Failure

Edit your existing pipeline YAML file (`.azure-pipelines/ci.yml`):

```yaml
steps:
- script: node app.js
  displayName: 'Run App'
````

Now **remove the `NodeTool@0` task** on purpose (to simulate failure).

Push this change to your repo and watch the pipeline fail.

---

### 🔍 Observe the Failure

Go to:
**Pipelines → Runs → Failed Job → Logs**

You’ll see an error:

```
##[error]node: command not found
```

### 🧠 Root Cause

Azure agent doesn’t have Node.js pre-installed.

---

### 🛠️ Fix It

Add the NodeTool task back:

```yaml
- task: NodeTool@0
  inputs:
    versionSpec: '18.x'
  displayName: 'Install Node.js'
```

Commit & push again:

```bash
git add .
git commit -m "fix: added NodeTool task"
git push
```

✅ Pipeline runs successfully again.

---

## 🧱 2. Check Logs in Detail

In the failed pipeline:

* Expand each step → click **View raw logs**
* Note where the red error starts
* Copy the error text for debugging

**Pro Tip:**
Use the search box in logs → search for `"##[error]"`
It jumps directly to the failed command.

---

# 🧩 SECTION 2 — Authentication & Permission Issues

---

## 🧰 1. Simulate a Service Connection Error

In Azure DevOps:

1. Go to **Project Settings → Service Connections**
2. Temporarily disable or delete your Azure connection
3. Run a deployment pipeline again.

You’ll get:

```
##[error]Failed to obtain access token for service principal
```

---

### 🧠 Root Cause

Pipeline cannot authenticate with Azure.

---

### 🛠️ Fix It

Recreate the service connection:

1. Project Settings → Service Connections → **New Service Connection**
2. Select **Azure Resource Manager → Service principal (automatic)**
3. Choose subscription & resource group
4. Name it: `MyAzureServiceConnection`
5. Save → Re-run pipeline.

✅ Deployment now works.

---

## 🧾 2. Test Permission Levels

Try deploying with a limited account that lacks access to the resource group.
You’ll see:

```
##[error]AuthorizationFailed: The client does not have authorization to perform action...
```

**Fix:**
Run in Azure CLI:

```bash
az role assignment create --assignee <client-id> --role Contributor --scope /subscriptions/<sub-id>/resourceGroups/<rg-name>
```

✅ Ensure your service principal has at least **Contributor** rights.

---

# 🧩 SECTION 3 — Repository & Merge Issues

---

## ⚙️ 1. Simulate a Merge Conflict

1. Checkout your `develop` branch:

   ```bash
   git checkout develop
   echo "<p>From develop branch</p>" > index.html
   git add .
   git commit -m "develop: added index"
   git push
   ```

2. Now switch to feature branch:

   ```bash
   git checkout -b feature/homepage
   echo "<p>From feature branch</p>" > index.html
   git add .
   git commit -m "feature: added index"
   git push -u origin feature/homepage
   ```

3. Create a PR (`feature/homepage` → `develop`) in Azure DevOps.
   You’ll see:
   ❌ “Merge conflicts detected”

---

### 🛠️ Fix Locally

```bash
git fetch origin
git checkout feature/homepage
git merge origin/develop
```

Open the file and resolve the conflict:

```html
<p>From develop branch + From feature branch</p>
```

Then:

```bash
git add index.html
git commit -m "resolve merge conflict"
git push
```

✅ The PR will automatically update and become mergeable.

---

## 🧩 2. Branch Protection Test

Try pushing directly to main:

```bash
echo "Unauthorized test" >> test.txt
git add .
git commit -m "try direct push"
git push origin main
```

You’ll see:

```
remote: error: Pushes to this branch are not permitted
```

✅ Branch policies working correctly.
You must use Pull Requests for merging.

---

# 🧩 SECTION 4 — Artifact & Deployment Errors

---

## ⚙️ 1. Simulate Missing Artifact

Edit your CD pipeline file and purposely rename the artifact:

```yaml
resources:
  pipelines:
    - pipeline: ciPipeline
      source: WrongPipelineName   # incorrect name
```

Run the pipeline — it fails:

```
##[error]Resource not found: Pipeline WrongPipelineName
```

---

### 🧠 Root Cause

Artifact name in CD doesn’t match the CI pipeline name.

---

### 🛠️ Fix It

Check your actual CI pipeline name in Azure DevOps (e.g., `ADO-CI`):

```yaml
resources:
  pipelines:
    - pipeline: ciPipeline
      source: ADO-CI
      trigger: true
```

✅ Run again — success!

---

## 🚀 2. Fix Deployment Path Issues

If you see:

```
##[error]Error: Package not found at path
```

### 🧭 Cause

Wrong artifact path.

### 🛠️ Fix:

Ensure the correct artifact path:

```yaml
package: '$(Pipeline.Workspace)/**/drop/*.zip'
```

✅ Always confirm artifact name = "drop" (from your CI publish step).

---

# 🧩 SECTION 5 — Agent & Queue Troubleshooting

---

## ⚙️ 1. Simulate Queue Delay

Run multiple pipelines simultaneously (3–4 runs).
You may see:

```
Waiting for an available agent...
```

### 🧠 Cause

No available agents in the pool.

### 🛠️ Fix:

1. Project Settings → Agent Pools → Add another agent
2. Or create your own self-hosted agent:

```bash
mkdir myagent && cd myagent
curl -O https://vstsagentpackage.azureedge.net/agent/3.236.1/vsts-agent-linux-x64-3.236.1.tar.gz
tar zxvf vsts-agent-linux-x64-3.236.1.tar.gz
./config.sh
./run.sh
```

✅ Self-hosted agents skip the queue!

---

## ⚙️ 2. Job Timeout

Error:

```
The job exceeded the maximum execution time of 60 minutes
```

### 🧭 Cause

* Long-running scripts or infinite loops

### 🛠️ Fix:

Increase job timeout in YAML:

```yaml
timeoutInMinutes: 120
```

Or optimize tasks:

* Add caching
* Split long jobs into smaller ones

---

# 🧩 SECTION 6 — Variable & Secret Issues

---

## 🧰 1. Missing Variable

Remove a variable from your YAML (simulate error):

```yaml
variables:
  envName: $(missingValue)
```

Error:

```
##[error]The value for the variable 'missingValue' was not found.
```

### 🛠️ Fix:

Define variable in:

* YAML file
* Or go to **Pipelines → Library → Variable groups → Add variable**

✅ Keep sensitive values as “Secret”.

---

## 🔐 2. Secret Leaked in Logs (Simulate Safely)

Add this line to YAML:

```yaml
- script: echo "API key is $(apiKey)"
```

Output shows:

```
API key is ***
```

✅ Azure DevOps automatically hides secrets.
Never hardcode them or print them directly.

---

# 🧩 SECTION 7 — Using Diagnostic Tools

---

### 🧰 Enable System Debug Logs

In your YAML:

```yaml
variables:
  system.debug: true
```

Run pipeline → you’ll get **verbose logs** showing:

* Environment variables
* Detailed step execution
* API requests

### 🧠 Tip:

Search logs for `##[error]` or `##[warning]` to quickly locate problems.

---

# 🧩 SECTION 8 — Final Troubleshooting Checklist

| Issue Type       | Example Error             | Quick Fix                              |
| ---------------- | ------------------------- | -------------------------------------- |
| Missing tool     | “command not found”       | Add correct task (NodeTool, UseDotNet) |
| Long queue       | “Waiting for agent”       | Use self-hosted agent                  |
| Unauthorized     | “AuthorizationFailed”     | Fix service connection permissions     |
| Merge conflict   | “Automatic merge failed”  | Pull, resolve, push                    |
| Missing artifact | “Resource not found”      | Match artifact name                    |
| Timeout          | “Job exceeded time limit” | Optimize or increase timeout           |
| Secret issue     | “Variable not found”      | Define or secure in variable group     |

---

# 🧠 SECTION 9 — Verify Your Troubleshooting Skills

✅ After following all steps:

* You fixed at least one pipeline failure.
* You resolved a merge conflict.
* You tested and repaired a service connection.
* You read and understood build logs.
* You verified your fixes worked.

🎉 Congratulations — You now know how to debug and fix **common Azure DevOps issues like a pro!**

---

# 📘 Summary

| Area        | Skill Gained                |
| ----------- | --------------------------- |
| Pipelines   | Read logs, fix build errors |
| Permissions | Reconnect & authorize Azure |
| Agents      | Reduce queue time           |
| Repos       | Merge conflict resolution   |
| Variables   | Secure secrets              |
| Artifacts   | Fix missing path & naming   |
| Debugging   | Use logs, system.debug mode |

---

✅ **You have completed the Troubleshooting Practical Lab.**
You’re now ready to handle real Azure DevOps errors confidently in live projects!

```

---

Would you like me to generate a **summary “Day-wise consolidation file”** that combines all four `.md` lessons (Branching, Release, Performance, and Troubleshooting) into one easy-to-revise study file for your EY ADO session?
```
