Here’s a well-organized list of **advanced Git & GitHub commands** for developers who already know the basics.

---

## 🚀 Git & GitHub Advanced Commands

---

### 🔁 1. `git stash` – Save Work-in-Progress Temporarily

```bash
git stash
```

Temporarily hides changes without committing. Useful when switching branches.

```bash
git stash pop      # Reapply the stashed changes
git stash list     # View stashed entries
```

---

### 🌳 2. `git rebase` – Reapply Commits on Top of Another Base Tip

```bash
git checkout feature
git rebase main
```

Moves your feature branch commits to the top of `main`. Keeps history cleaner than merge.

> ⚠️ Use with caution if you're working with others on the same branch.

---

### 🧼 3. `git reset` – Undo Commits

```bash
git reset --soft HEAD~1    # Undo last commit, keep changes staged
git reset --mixed HEAD~1   # Undo last commit, keep changes
git reset --hard HEAD~1    # Undo everything permanently
```

> **Hard reset is destructive** and can't be undone easily.

---

### 🧪 4. `git cherry-pick` – Apply a Specific Commit from Another Branch

```bash
git cherry-pick <commit-hash>
```

Brings a specific commit from one branch to another.

---

### 🧾 5. `git reflog` – View History of All Git Actions

```bash
git reflog
```

Lets you see all HEAD movements. Useful for recovering lost commits.

---

### 🧬 6. `git bisect` – Find the Commit That Introduced a Bug

```bash
git bisect start
git bisect bad
git bisect good <commit-hash>
```

Helps isolate which commit caused a bug using binary search.

---

### 🔗 7. `git submodule` – Track Sub-Projects Inside a Repo

```bash
git submodule add https://github.com/user/project.git subfolder/
```

Useful for modular codebases or libraries.

---

### 🔐 8. `git config --global` – Advanced Config Settings

```bash
git config --global alias.co checkout
git config --global user.name "Your Name"
git config --global user.email "email@example.com"
```

Create aliases for frequent commands.

---

### 📦 9. `git tag` – Tag Specific Commits for Releases

```bash
git tag v1.0.0
git push origin v1.0.0
```

Used for versioning your project.

---

### ☁️ 10. GitHub-Specific Commands

```bash
# Clone repo
git clone https://github.com/user/repo.git

# Create and switch to a branch
git checkout -b new-feature

# Push a new branch to GitHub
git push -u origin new-feature

# View remote URLs
git remote -v

# Change remote URL
git remote set-url origin https://new-url.git
```

---

### ✅ Bonus Tips

* Use `.gitignore` to prevent unwanted files from being tracked.
* Use `git log --oneline --graph` for a visual commit history.
* Use GitHub Desktop or `gh` CLI for easier PR and repo management.

---

### 📘 Recommended for Advanced Learners

* GitHub CLI (`gh`)
* Git hooks
* Signed commits (`git commit -S`)
* Worktrees (`git worktree`) for multiple working directories

---
