---

# Real-World Project Example with Version Control Workflows

### Project: **Simple Blog Application**

**Tech stack:**

* Backend: Node.js with Express
* Frontend: React
* Database: SQLite or MongoDB (optional)

---

## Workflow Overview

1. **Setup Repo & Branches**
2. **Feature Development on Branches**
3. **Pull Requests for Review**
4. **Merging & Conflict Resolution**
5. **Tagging & Releases**
6. **Collaboration with Issues and Project Boards**

---

## Step-by-step Workflow

### Step 1: Initialize & Setup Repo

* Create a new GitHub repository: `simple-blog-app`
* Clone locally:

  ```bash
  git clone https://github.com/username/simple-blog-app.git
  cd simple-blog-app
  ```
* Create main branches:

  ```bash
  git checkout -b develop   # main development branch
  git push -u origin develop
  ```
* Protect `main` branch on GitHub (no direct commits allowed)

---

### Step 2: Branching for Features

* Start new feature branch from `develop`:

  ```bash
  git checkout develop
  git pull origin develop
  git checkout -b feature/user-auth
  ```
* Work on user authentication functionality

---

### Step 3: Commit & Push Often

* Make atomic commits:

  ```bash
  git add .
  git commit -m "feat: add user registration form"
  git push -u origin feature/user-auth
  ```

---

### Step 4: Create Pull Request (PR)

* Go to GitHub and create PR from `feature/user-auth` → `develop`
* Add description: What feature, what was changed
* Request code reviews from teammates
* Link relevant issues (e.g., fixes #12)

---

### Step 5: Review & Merge PR

* Reviewer comments or requests changes
* Developer updates branch:

  ```bash
  git checkout feature/user-auth
  git pull origin develop   # keep branch updated
  # fix issues
  git add .
  git commit -m "fix: handle validation errors"
  git push
  ```
* Once approved, merge PR via GitHub UI
* Delete feature branch after merging

---

### Step 6: Keep develop Updated & Prepare for Release

* Periodically merge `develop` into `main` for stable releases:

  ```bash
  git checkout main
  git pull origin main
  git merge develop
  git push origin main
  ```
* Tag release:

  ```bash
  git tag -a v1.0.0 -m "Release version 1.0.0"
  git push origin v1.0.0
  ```

---

### Step 7: Collaboration and Issue Tracking

* Use GitHub Issues to report bugs and request features
* Assign issues to developers and track progress on Project boards
* Link commits and PRs to issues by mentioning issue numbers in commit messages or PR descriptions (e.g., `fixes #15`)

---

## Summary of Git Commands in Workflow

| Action              | Command                                                        |
| ------------------- | -------------------------------------------------------------- |
| Clone repo          | `git clone https://github.com/username/repo.git`               |
| Create branch       | `git checkout -b branch-name`                                  |
| Switch branch       | `git checkout branch-name`                                     |
| Stage changes       | `git add .` or `git add filename`                              |
| Commit changes      | `git commit -m "message"`                                      |
| Push branch         | `git push -u origin branch-name`                               |
| Pull latest changes | `git pull origin branch-name`                                  |
| Merge branches      | `git checkout target-branch` <br> `git merge source-branch`    |
| Tag a release       | `git tag -a v1.0.0 -m "message"` <br> `git push origin v1.0.0` |

---

### Bonus: Handling Merge Conflicts

* When merging, if conflicts happen:

  ```bash
  # Git will indicate files with conflicts
  # Open conflicted files, edit, save fixes
  git add conflicted-file
  git commit
  ```

---
