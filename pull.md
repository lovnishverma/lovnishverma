To pull changes from a GitHub repository to your local machine, use the `git pull` command. Here's how:

---

### **Step 1: Ensure Git Is Installed**
Check if Git is installed on your machine:

```bash
git --version
```

If not, [install Git](https://git-scm.com/).

---

### **Step 2: Navigate to Your Local Repository**
Go to the local directory of the project where you want to pull updates:

```bash
cd /path/to/your/project
```

---

### **Step 3: Verify Remote Repository**
Make sure your local repository is connected to the correct remote repository. Check the remote URL:

```bash
git remote -v
```

If not set, add the remote repository:

```bash
git remote add origin https://github.com/your-username/your-repo.git
```

---

### **Step 4: Pull Changes**
Pull the latest changes from the remote repository:

```bash
git pull origin main
```

> Replace `main` with the branch name you're working on if it's different (e.g., `master`, `dev`, etc.).

---

### **Step 5: Resolve Merge Conflicts (If Any)**
If there are conflicts during the pull, Git will notify you. Open the conflicting files, resolve the conflicts, and mark them as resolved:

1. **Resolve conflicts manually** in the files.
2. Mark the files as resolved:

   ```bash
   git add <file-name>
   ```

3. Complete the merge:

   ```bash
   git commit -m "Resolve merge conflicts"
   ```

---

### **Optional Commands for Clean Up**
- **View Current Branch**:

  ```bash
  git branch
  ```

- **Switch Branch** (if you want to pull from a different branch):

  ```bash
  git checkout <branch-name>
  ```

---
