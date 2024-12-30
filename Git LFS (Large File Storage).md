GitHub imposes a 100MB file size limit for files in a repository and recommends using Git LFS (Large File Storage) for files larger than 50MB. Here's how to push files larger than 50MB to your GitHub repository:

---

### **Step 1: Install Git LFS**
Install Git Large File Storage (LFS) on your system:

- **Linux**:  
  Use your package manager. For example:  
  ```bash
  sudo apt install git-lfs
  ```

- **macOS**:  
  Use Homebrew:  
  ```bash
  brew install git-lfs
  ```

- **Windows**:  
  Download the installer from the [Git LFS website](https://git-lfs.github.com/).

---

### **Step 2: Initialize Git LFS in Your Repository**
Navigate to your repository and initialize Git LFS:

```bash
cd /path/to/your/repository
git lfs install
```

---

### **Step 3: Track Large Files**
Specify which files Git LFS should track. For example, to track a file named `large-file.zip`:

```bash
git lfs track "large-file.zip"
```

This will add a `.gitattributes` file to your repository.

If you want to track files by type (e.g., all `.zip` files):

```bash
git lfs track "*.zip"
```

---

### **Step 4: Add and Commit the Files**
Add the files to the Git staging area and commit the changes:

```bash
git add .gitattributes
git add large-file.zip
git commit -m "Add large file with Git LFS"
```

---

### **Step 5: Push to GitHub**
Push your changes to the GitHub repository:

```bash
git push origin main
```

Replace `main` with the appropriate branch name.

---

### **Step 6: Verify on GitHub**
Check your repository on GitHub to ensure the large file has been successfully uploaded. GitHub LFS files are stored separately from the repository.

---

### **Important Notes**
1. **Quota Limits**: Git LFS provides a limited amount of free storage and bandwidth. You may need to purchase additional storage for larger projects.
2. **Collaborators**: Ensure collaborators have Git LFS installed before cloning the repository to handle large files properly.
3. **File Removal**: To remove a large file already committed to Git history, you may need to rewrite history with tools like `git filter-repo` or `BFG Repo Cleaner`.
