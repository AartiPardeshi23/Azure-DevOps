# 💾 Backup and Restore – Azure DevOps

## 🎯 Objective
Learn how to safeguard your Azure DevOps organization and recover from data loss.

---

## Step 1: Azure DevOps Auto-Backup
Microsoft automatically:
- Replicates data across data centers
- Backs up Repos, Pipelines, Boards, and Artifacts
No manual backup needed for internal systems.

---

## Step 2: Manual Project Data Export
- Go to **Boards → Queries**
- Run “All Work Items”
- Click **Export to CSV** → Download file.

💾 Stores all tasks, bugs, and work items locally.

---

## Step 3: Clone Git Repo
1. Open GitHub repo: `https://github.com/AartiPardeshi23/React_Development_Learning`
2. Click **Code → Copy HTTPS link**
3. Clone using **VS Code** or **GitHub Desktop**
   - Keeps a local copy of your project.

---

## Step 4: Export Pipeline YAML
1. Go to **Pipelines → [Your Pipeline]**
2. Click **View YAML**
3. Copy content → Save as `azure-pipelines.yml`

---

## Step 5: Restore Deleted Projects
1. Go to **Organization Settings → Projects**
2. Click **Deleted Projects**
3. Find the deleted one → Click **Restore**

Restores all repos, pipelines, and boards (available for 28 days).

---

## Step 6: Export User and Audit Data
- **Users → Export CSV**
- **Auditing → Export CSV**
Helps rebuild roles and investigate actions later.

---

## ✅ Summary

| Type | How | Why |
|------|-----|-----|
| Work items | Export CSV | Local tracking |
| Code | Clone repo | Source backup |
| Pipelines | Save YAML | Restore build |
| Users | Export CSV | Recreate permissions |
| Projects | Restore | Full recovery |
