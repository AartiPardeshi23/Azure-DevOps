# Azure DevOps — Branching and Merging Strategies

## 🧠 Concept
Branching allows multiple developers to work in parallel without breaking the main code.  
Merging combines their work safely back into main branches.

### Common Strategy (Git Flow)
- **main** → Always stable, production-ready.
- **develop** → Integration branch for all completed features.
- **feature/** → For individual tasks or features.
- **release/** → For final testing before going live.
- **hotfix/** → For emergency production fixes.

---

## 🧪 Practical Lab

### 1. Initialize repository
```bash
mkdir ado-demo && cd ado-demo
git init
echo "# ADO Demo Project" > README.md
git add .
git commit -m "Initial commit"
git branch -M main
git remote add origin https://dev.azure.com/<ORG>/<PROJECT>/_git/<REPO>
git push -u origin main
Sure ✅ — here’s the **complete ready-to-copy Markdown file** for **Topic 1: Branching and Merging Strategies** —
it includes both **Theory** and **Practical (Hands-on)** in a single `.md` file.


# 🌿 Azure DevOps — Branching and Merging Strategies

## 📘 Theory + Hands-on Practical Guide  
*(Designed for beginners and slow learners — explained simply and step-by-step)*

---

## 🧭 1. What Is Branching?

Branching means creating a **separate copy of your code** to work on changes **without disturbing the main codebase**.

Think of it like working on a new feature on your own desk, while the main project continues on the main desk.

Each branch represents:
- A version of your code
- A specific purpose (feature, bug fix, release, etc.)

---

## 💡 Why We Use Branches

| Reason | Example |
|--------|----------|
| 🧩 Isolation | Work on “login feature” without touching live code |
| 🧑‍🤝‍🧑 Collaboration | Multiple developers can code at once |
| 💥 Risk Control | Mistakes stay in your branch until reviewed |
| 🧹 Clean History | Each feature has its own commit history |

---

## 🌳 2. Common Branching Models

### A) **Main-only (Simple Model)**  
Used for small teams or personal projects.

```

main
└── commits → deploy directly

```
✅ Easy  
❌ Not safe for multiple developers (any bug affects production)

---

### B) **Feature Branch Model**
Each new feature or bug fix happens in its own branch.

```

main
├── feature/login
├── feature/signup
└── bugfix/ui-color

```
✅ Keeps features separate  
❌ Merges can get complex if many developers work at once

---

### C) **Git Flow (Standard Practice)**
A structured model with defined purposes for each branch.

```

main
└── develop
├── feature/login
├── feature/payment
└── release/v1.0
main ← hotfix/critical-fix

````

**Branch Roles:**

| Branch | Purpose |
|---------|----------|
| `main` | Production-ready code |
| `develop` | Integration / testing branch |
| `feature/*` | New feature work |
| `release/*` | Prepare code for deployment |
| `hotfix/*` | Urgent production fix |

✅ Stable workflow for large teams  
✅ Allows parallel development  
❌ Slightly more complex to manage

---

## 🔄 3. What Is Merging?

Merging means **combining changes** from one branch into another (e.g., merging `feature/login` into `develop`).

Example:
```bash
git checkout develop
git merge feature/login
````

---

## ⚔️ 4. Merge Conflicts — What & Why

When two branches edit the same lines in a file, Git can’t decide whose change is correct.

Example conflict:

```text
<<<<<<< HEAD
console.log("Hello from develop");
=======
console.log("Hello from feature");
>>>>>>> feature/login
```

Fix it manually, then:

```bash
git add .
git commit -m "resolve merge conflict"
```

---

## 🧰 5. Pull Requests (PRs)

Pull Requests are **formal requests to merge your branch** into another through Azure DevOps.

Steps:

1. Create a branch (e.g. `feature/login`)
2. Push it to remote
3. Go to Azure DevOps → Repos → Branches → Create Pull Request
4. Add reviewers & description
5. Wait for approval and merge

---

## 🛡️ 6. Branch Policies (Protection Rules)

Branch policies prevent accidental merges or broken builds.

**In Azure DevOps:**

* Require PRs (no direct push to `main`)
* Require reviewers (1 or 2)
* Require successful CI build
* Require linked work items
* Limit merge types (e.g., Squash merge)

Purpose → Keeps production branch stable and reviewed.

---

## 🧱 7. Merge Strategies in Azure DevOps

| Strategy         | Description                   | Example History        |
| ---------------- | ----------------------------- | ---------------------- |
| **Merge commit** | Adds one merge commit         | Full history preserved |
| **Squash**       | Combines all commits into one | Clean, short history   |
| **Rebase**       | Makes history look linear     | No merge commits       |

✅ Best Practice → Use **Squash Merge** for clean main branch.

---

## 🧩 8. Naming Conventions

| Type    | Example                 |
| ------- | ----------------------- |
| Feature | `feature/add-login-api` |
| Bug Fix | `bugfix/fix-null-error` |
| Release | `release/v1.0.0`        |
| Hotfix  | `hotfix/payment-bug`    |

Use lowercase, hyphens, and prefix type.

---

## 🧭 9. Summary

1. Protect `main` branch
2. Create new branches for every change
3. Commit small and clear
4. Merge via PRs
5. Delete old branches
6. Resolve conflicts before merging
7. Use Squash merge for clean history

---

## 🧠 10. Quick Quiz

1. Why use branching?
2. What’s the purpose of `develop`?
3. How do Pull Requests help?
4. What’s a merge conflict?
5. Why use branch policies?

---

# 🧪 PRACTICAL LAB — Branching & Merging in Azure DevOps

---

## ⚙️ Step 1: Setup

### Prerequisites

✅ Git installed (`git --version`)
✅ Azure DevOps account
✅ Project + Repository created

---

## 🪜 Step 2: Clone the Repository

Azure DevOps → **Repos → Clone → HTTPS URL**
Then:

```bash
cd Desktop
git clone https://dev.azure.com/ORG/PROJECT/_git/ado-demo
cd ado-demo
```

---

## 🌱 Step 3: Create `develop` Branch

```bash
git checkout main
git checkout -b develop
git push -u origin develop
```

✅ Check in Azure DevOps → Repos → Branches

---

## 🌿 Step 4: Create Feature Branch

```bash
git checkout develop
git checkout -b feature/login-page
echo "<h1>Login Page Feature</h1>" > login.html
git add login.html
git commit -m "add login page feature"
git push -u origin feature/login-page
```

---

## 🔁 Step 5: Create Pull Request

1. Azure DevOps → Repos → Branches → `feature/login-page` → **Create PR**
2. Target branch: `develop`
3. Add title + description
4. Add reviewer
5. Click **Create**

---

## ⚙️ Step 6: Add Branch Policy

1. Project Settings → Repos → Branches → select `main`
2. Add Policy:

   * Require 1 reviewer
   * Require successful build
   * Limit to **Squash merge**
3. Save

---

## 🧩 Step 7: Merge PR

After review & build:

* Click **Complete PR**
* Choose **Squash Merge**
* Delete source branch ✅

✅ Feature merged to `develop`

---

## ⚠️ Step 8: Simulate Merge Conflict

### In `develop`:

```bash
git checkout develop
echo "<p>Welcome to our site</p>" > index.html
git add index.html
git commit -m "add welcome message"
git push
```

### In `feature/home-page`:

```bash
git checkout -b feature/home-page
echo "<p>Hello World!</p>" > index.html
git add index.html
git commit -m "add hello world message"
git push -u origin feature/home-page
```

Now both modified the same file.

---

## 💥 Step 9: Resolve Conflict

Azure DevOps PR → conflict warning.
Fix locally:

```bash
git fetch origin
git checkout feature/home-page
git merge origin/develop
```

Fix the conflict manually:

```html
<p>Welcome to our site — Hello World!</p>
```

Then:

```bash
git add index.html
git commit -m "resolve merge conflict"
git push
```

✅ PR now mergeable → Complete merge.

---

## 🧹 Step 10: Clean Branches

```bash
git branch -d feature/login-page
git push origin --delete feature/login-page

git branch -d feature/home-page
git push origin --delete feature/home-page
```

---

## 💬 Step 11: Merge Develop → Main

```bash
git checkout main
git merge develop
git push origin main
```

or via PR (`develop` → `main`)

---

## 🧭 Step 12: Test Branch Policy

Try to push directly:

```bash
echo "test" >> test.txt
git add test.txt
git commit -m "test direct push"
git push origin main
```

Result:

```
remote: error: Pushes to this branch are not permitted
```

✅ Policy works!

---

## 📘 Summary Table

| Step | Action                | Tool         |
| ---- | --------------------- | ------------ |
| 1    | Clone repo            | Git          |
| 2    | Create develop        | Git          |
| 3    | Create feature branch | Git          |
| 4    | Push branch           | Git          |
| 5    | Create PR             | Azure DevOps |
| 6    | Add branch policy     | Azure DevOps |
| 7    | Merge PR              | Azure DevOps |
| 8    | Resolve conflict      | Git          |
| 9    | Protect main          | Azure DevOps |

---

## 🧠 Practice Quiz

1. Which command creates a new branch?
2. How to merge branches?
3. What’s the purpose of a Pull Request?
4. What is a merge conflict?
5. Why should `main` be protected?

---

## ✅ Expected Output

* Repo has `main` and `develop`
* PR merges only via review
* Direct push to `main` blocked
* Conflicts resolved manually
* Clean, readable history

🎉 **You’ve completed the Branching & Merging Strategies lab successfully!**

```

---

Would you like me to generate the next Markdown file  
📘 `02_Release_Management_Best_Practices.md` (theory only next)?
```
