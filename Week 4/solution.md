### **1️⃣ Setup – Create a New Repo**

1. On GitHub → Create a new repo:

   ```
   two-tier-flask-app
   ```
2. Clone it locally:

   ```bash
   git clone https://github.com/vaishnavitandekar/two-tier-flask-app.git
   cd two-tier-flask-app
   ```

---

### **2️⃣ Task 1 – Pull Requests**

**Goal:** Create & merge a feature branch via PR.

1. Create a `feature-branch`:

   ```bash
   git checkout -b feature-branch
   echo "Feature: Added Hello World" > feature.txt
   git add .
   git commit -m "Added Hello World feature"
   ```
2. Push:

   ```bash
   git push origin feature-branch
   ```
3. On GitHub → Open PR → Request review → Merge to `main`.

<img width="1366" height="731" alt="Image" src="https://github.com/user-attachments/assets/09f11dbe-ec3a-4963-819b-30d2e1b6b5b9" />
<img width="1366" height="768" alt="Image" src="https://github.com/user-attachments/assets/ad120d7e-7b5c-4ce7-a4e4-1283bf77b5c7" />
<img width="1366" height="768" alt="Image" src="https://github.com/user-attachments/assets/43c2846e-11d1-4631-a734-43f4f46c9842" />
<img width="1366" height="768" alt="Image" src="https://github.com/user-attachments/assets/adde7597-7292-4cb2-810b-25f2fae7b693" />

---

### **3️⃣ Task 2 – Reset & Revert**

**Goal:** Practice all reset types + revert.

1. Create a wrong commit:

   ```bash
   echo "Wrong code" > wrong.txt
   git add .
   git commit -m "Mistaken commit"
   ```
2. **Soft Reset**:

   ```bash
   git reset --soft HEAD~1
   ```

   → Commit undone, changes still staged.
3. Commit again, then try **Mixed Reset**:

   ```bash
   git commit -m "Mistaken commit again"
   git reset --mixed HEAD~1
   ```

   → Changes kept, unstaged.
4. Commit again, then try **Hard Reset**:

   ```bash
   git commit -m "Mistaken commit yet again"
   git reset --hard HEAD~1
   ```

   → Changes removed completely.
5. Commit again, push, then **Revert**:

   ```bash
   git revert HEAD
   git push origin main
   ```

<img width="1366" height="768" alt="Image" src="https://github.com/user-attachments/assets/1a97fac1-a747-4edb-babc-b1518d153e6d" />

---

### **4️⃣ Task 3 – Stashing**

**Goal:** Save & restore incomplete work.

1. Create changes without committing:

   ```bash
   echo "Temporary change for stash" > temp.txt
   git add temp.txt
   git stash
   ```
2. Check stash list:

   ```bash
   git stash list
   ```
3. Switch branch, then restore:

   ```bash
   git checkout -b stash-branch
   git stash pop  # or git stash apply
   ```

<img width="1366" height="768" alt="Image" src="https://github.com/user-attachments/assets/1a97fac1-a747-4edb-babc-b1518d153e6d" />


---

### **5️⃣ Task 4 – Cherry-Picking**

**Goal:** Take a commit from one branch to another.

1. On `bugfix-branch`:

   ```bash
   git checkout -b bugfix-branch
   echo "Bug fixed" > bug.txt
   git add .
   git commit -m "Fixed a bug"
   git push origin bugfix-branch
   ```
2. Go to `main` branch:

   ```bash
   git checkout main
   ```
3. Find commit hash:

   ```bash
   git log --oneline
   ```
4. Cherry-pick it:

   ```bash
   git cherry-pick <commit-hash>
   ```

<img width="1366" height="768" alt="Image" src="https://github.com/user-attachments/assets/a9c7800b-2dd3-4545-a87d-96cd6135f07a" />
<img width="1366" height="768" alt="Image" src="https://github.com/user-attachments/assets/8df4d44f-39b6-456a-8b72-4d967c2fc6f5" />
<img width="1366" height="768" alt="Image" src="https://github.com/user-attachments/assets/7c175850-5eee-465e-a7ef-a6aabfdf08e3" />

---

### **6️⃣ Task 5 – Rebasing**

**Goal:** Update feature branch with main’s latest changes without merge commits.

1. Create a `rebase-branch`:

   ```bash
   git checkout -b rebase-branch
   echo "Rebase test" > rebase.txt
   git add .
   git commit -m "Work for rebase"
   ```
2. Switch to `main` and add a commit:

   ```bash
   git checkout main
   echo "Main branch update" > mainupdate.txt
   git add .
   git commit -m "Main update"
   ```
3. Go back to `rebase-branch`:

   ```bash
   git checkout rebase-branch
   git fetch origin main
   git rebase origin/main
   ```
4. Resolve conflicts if needed:

   ```bash
   git add .
   git rebase --continue
   ```

<img width="1366" height="768" alt="Image" src="https://github.com/user-attachments/assets/ff30b35c-bab0-4982-b8ac-45b6d3c22ce7" />
<img width="1366" height="768" alt="Image" src="https://github.com/user-attachments/assets/04c9479c-0606-488a-bb45-78f72bab1494" />

---

### **7️⃣ Task 6 – Branching Strategies Simulation**

**Goal:** Simulate Git Flow, GitHub Flow, and Trunk-Based Development.

1. **Git Flow example:**

   ```bash
   git branch develop
   git checkout -b feature/login
   git checkout -b hotfix/urgent-fix
   ```
2. **GitHub Flow example:**

   * Always branch from `main`
   * Small PRs, merge quickly
3. **Trunk-Based Development example:**

   * Commit directly to main or very short-lived branches

📌 **Document in solution.md** pros & cons of each strategy.

---

### **8️⃣ Final Commit & Push**

Once all tasks are done:

```bash
git add solution.md
git commit -m "Completed Week 4 Git & GitHub Advanced Challenge"
git push origin main
```

Then open **one final PR** with all work.
