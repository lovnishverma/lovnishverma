To push a project to GitHub, follow these steps:

---

### **Step 1: Install Git**
Ensure that Git is installed on your system. You can check by running the following command in your terminal:

```bash
git --version
```

If Git is not installed, download and install it from [git-scm.com](https://git-scm.com/).

---

### **Step 2: Create a GitHub Repository**
1. Go to [GitHub](https://github.com/) and log in.
2. Click the **New** button or `+` icon at the top right of the page.
3. Name your repository and optionally provide a description.
4. Choose visibility: **Public** or **Private**.
5. Click **Create repository**.

---

### **Step 3: Initialize Your Project with Git**
1. Navigate to your project folder in the terminal:

   ```bash
   cd /path/to/your/project
   ```

2. Initialize Git:

   ```bash
   git init
   ```

3. Add all files to the Git staging area:

   ```bash
   git add .
   ```

4. Commit the changes:

   ```bash
   git commit -m "Initial commit"
   ```

---

### **Step 4: Link Your Local Repository to GitHub**
1. Copy the remote URL of your GitHub repository (from the GitHub page).
2. Add the remote origin:

   ```bash
   git remote add origin https://github.com/your-username/your-repo.git
   ```

3. Verify the remote URL:

   ```bash
   git remote -v
   ```

---

### **Step 5: Push Your Project to GitHub**
1. Push the code to GitHub:

   ```bash
   git branch -M main
   git push -u origin main
   ```

2. If prompted, enter your GitHub username and password (or personal access token if you have 2FA enabled).

---

### **Step 6: Verify on GitHub**
Go to your repository URL on GitHub to verify that the files have been uploaded.

---

### **Future Updates**
For subsequent changes, use:

![image](https://github.com/user-attachments/assets/1815b4f0-67da-4bd2-adf8-e25f39833e60)


1. Stage the changes:

   ```bash
   git add .
   ```

2. Commit the changes:

   ```bash
   git commit -m "Your commit message"
   ```

3. Push to GitHub:

   ```bash
   git push
   ``` 

---
