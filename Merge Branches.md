Merging branches is a key part of Git workflows — it's how you bring changes from one branch (e.g., a feature branch) into another (typically `main` or `master`).

---

## 🔀 How to Merge Branches in Git

Let’s say you have:

* A `main` branch
* A `feature` branch where you made some changes

---

### ✅ 1. First, **switch to the branch** you want to merge *into* (e.g., `main`):

```bash
git checkout main
```

> 💡 Always merge *into* the target branch. You’ll pull changes *into this*.

---

### ✅ 2. Then, **merge the other branch** into it (e.g., `feature`):

```bash
git merge feature
```

This applies the changes from the `feature` branch into `main`.

---

### 📌 What Happens Now?

* If there are **no conflicts**, Git will auto-merge and create a **merge commit**.
* If there **are conflicts**, Git will pause and ask you to resolve them manually.

---

### 💥 Example Workflow:

```bash
# create and switch to feature branch
git checkout -b feature

# make some changes
echo "New feature" > feature.txt
git add .
git commit -m "Added feature"

# switch back to main
git checkout main

# merge feature into main
git merge feature
```

---

### 🛠 Handling Merge Conflicts (if any)

Git will show conflicted files like this:

```bash
CONFLICT (content): Merge conflict in file.txt
```

1. Open the file and resolve the conflict manually:

   ```plaintext
   <<<<<<< HEAD
   content from main
   =======
   content from feature
   >>>>>>> feature
   ```

2. After fixing, mark the file as resolved:

   ```bash
   git add file.txt
   ```

3. Then complete the merge:

   ```bash
   git commit
   ```

---

### 📤 Push the merged branch to GitHub:

```bash
git push origin main
```

---

### 📘 Summary Table

| Task                       | Command                              |
| -------------------------- | ------------------------------------ |
| Switch to main             | `git checkout main`                  |
| Merge feature into main    | `git merge feature`                  |
| Resolve conflicts (if any) | Edit file → `git add` → `git commit` |
| Push to GitHub             | `git push origin main`               |

---

