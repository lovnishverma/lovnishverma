To **revert changes in Git**, there are **multiple ways** depending on *what exactly you want to undo* — changes in a file, a commit, or pushing to remote.

---

## 🔁 Git Revert, Reset, and Checkout – How to Undo Changes

---

### 📁 1. Undo Unstaged Changes in a File (not yet added with `git add`)

```bash
git checkout -- filename
```

✅ This will discard all **unsaved changes** to that file and revert it to the **last committed version**.

> ⚠️ Cannot be undone! Use carefully.

---

### 📥 2. Unstage a File (after `git add`, but before `commit`)

```bash
git reset filename
```

✅ This removes the file from the staging area, but **keeps your edits** in the working directory.

---

### 🗑 3. Discard All Local Changes (not yet committed)

```bash
git reset --hard
```

✅ This **resets everything** to the last commit (working directory and staging area). All unsaved changes are lost!

---

### 🔙 4. Undo Last Commit (but keep changes in working directory)

```bash
git reset --soft HEAD~1
```

✅ This removes the last commit but keeps the changes so you can recommit with a new message.

---

### 🧼 5. Undo Last Commit and Discard Changes

```bash
git reset --hard HEAD~1
```

✅ This deletes the last commit and all related changes completely.

---

### 🔄 6. Revert a Commit (safe, used on shared repos)

```bash
git revert <commit_hash>
```

✅ This creates a new commit that **undoes** the effects of the specified commit (safe for public branches).

---

### 🧠 Tip for Teaching:

| Command                   | Use Case                      | Keeps Changes?  |
| ------------------------- | ----------------------------- | --------------- |
| `git checkout -- file`    | Revert single file            | ❌               |
| `git reset file`          | Unstage file                  | ✅               |
| `git reset --soft HEAD~1` | Undo commit, keep changes     | ✅               |
| `git reset --hard`        | Wipe all local changes        | ❌               |
| `git revert <hash>`       | Undo a commit (safe for team) | ✅ (with commit) |

---


