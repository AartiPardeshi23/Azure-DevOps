# 🧑‍💼 Managing Users and Permissions – Azure DevOps

## 🎯 Objective
Learn how to add, manage, and control user access in Azure DevOps using the portal (no CLI).

---

## Step 1: Open Organization Settings
1. Go to [https://dev.azure.com](https://dev.azure.com)
2. Sign in with your Microsoft account.
3. Select your organization (e.g., `ey-ado-org`).
4. Click the ⚙️ **Organization Settings** icon (bottom-left corner).

---

## Step 2: Add a User
1. In the left panel, click **Users**.
2. Click **Add users** (top-right corner).
3. Enter the user’s email (e.g., `testuser@example.com`).
4. Choose **Access Level**:
   - `Basic` → Developers/Testers
   - `Stakeholder` → View-only users (free)
5. Choose **Project** and **Group (Contributors)**.
6. Click **Add**.

✅ User appears with “Pending” status until they accept the invite.

---

## Step 3: Assign Permission Group
1. Go to **Organization Settings → Projects → [Your Project] → Security**.
2. Select a group:
   - `Project Readers`
   - `Project Contributors`
   - `Project Administrators`
3. Click **Add** → enter user’s email → **Save**.

---

## Step 4: Verify Permissions
1. Click the user’s name → open their permissions view.
2. Check each permission’s state:
   - `Allowed`, `Denied`, or `Inherited`.

---

## 🔐 Permission Summary

| Role | Permissions |
|------|--------------|
| **Reader** | View-only |
| **Contributor** | Edit work items, code, pipelines |
| **Administrator** | Manage project settings |
| **Org Owner** | Manage all projects, users, billing |

✅ Always assign **minimum required** permissions.
