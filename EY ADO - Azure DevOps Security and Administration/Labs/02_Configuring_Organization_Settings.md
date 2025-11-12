# ⚙️ Configuring Organization Settings – Azure DevOps

## 🎯 Objective
Understand and configure global Azure DevOps settings that affect all projects.

---

## Step 1: Access Organization Settings
1. Click ⚙️ **Organization Settings** (bottom-left of portal).
2. You’ll see sections: *Overview, Billing, Security, Policies, Process, Service connections*, etc.

---

## Step 2: General Settings
- **Overview** tab → shows:
  - Organization name
  - URL (e.g., `https://dev.azure.com/ey-ado-org`)
  - Region (e.g., Central India)
- You can rename or change settings if you’re the Org Owner.

---

## Step 3: Billing and Licenses
- Click **Billing**:
  - View users, license counts (Basic, Stakeholder)
  - Connect Azure subscription for billing (optional)
- Default: 5 free Basic users.

---

## Step 4: Security Policies
1. Navigate to **Security → Policies**
2. Toggle:
   - ✅ “Only owners can create new projects”
   - ✅ “Require email verification”
3. This prevents accidental project creation and enforces security.

---

## Step 5: Process Templates
- Click **Process** → view templates:
  - **Agile**, **Scrum**, **CMMI**, **Basic**
- You can clone and create a **custom process** to define your own work item types.

---

## Step 6: Service Connections
- Go to **Service connections** to connect Azure DevOps with:
  - Azure
  - GitHub
  - Docker Hub
  - AWS
- Used by pipelines for deployment automation.

✅ Now you know how to control your organization globally.
