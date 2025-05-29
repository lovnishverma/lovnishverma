# Merge Conflicts: What, Why, and How to Fix

---

## What is a Merge Conflict?

* A **merge conflict** happens when Git can’t automatically combine changes from two branches because the same part of the same file was changed in conflicting ways.
* It’s Git’s way of saying: **“I need your help deciding which change to keep.”**

---

## How Do Merge Conflicts Happen?

Imagine two developers working on the same file:

* **Developer A** edits line 10 of `app.js` to change the function behavior.
* **Developer B** edits the same line 10 of `app.js` but with different logic.

When you try to merge their branches, Git can’t decide which version to keep, so it raises a conflict.

### Common causes:

* Simultaneous edits to the same lines of code in different branches
* Deleting a file in one branch but modifying it in another
* Moving or renaming files differently in branches

---

## How to Identify Merge Conflicts?

When you do:

```bash
git merge branch-name
```

If there’s a conflict, Git will show a message:

```
Auto-merging filename.ext
CONFLICT (content): Merge conflict in filename.ext
Automatic merge failed; fix conflicts and then commit the result.
```

Git also marks the conflicting areas inside the file.

---

## How to Resolve Merge Conflicts?

### Step 1: Open the conflicted file(s)

Inside the conflicted files, Git inserts special markers:

```plaintext
<<<<<<< HEAD
Your changes (current branch)
=======
Incoming changes (branch being merged)
>>>>>>> branch-name
```

Example:

```javascript
function greet() {
<<<<<<< HEAD
  console.log("Hello from main branch!");
=======
  console.log("Hello from feature branch!");
>>>>>>> feature-branch
}
```

### Step 2: Decide what to keep

You have 3 options:

* Keep your version (above `=======`)
* Keep incoming version (below `=======`)
* Combine both versions manually if needed

Edit the file to remove the conflict markers (`<<<<<<<`, `=======`, `>>>>>>>`) and finalize the correct code.

---

### Step 3: Mark conflict as resolved and commit

```bash
git add conflicted-file.js
git commit -m "Resolved merge conflict in conflicted-file.js"
```

---

## Tips to Avoid Merge Conflicts

* Pull changes often before starting work:
  `git pull origin branch-name`
* Communicate with your team about what files you’re editing
* Work on separate features or parts of the codebase if possible
* Make small, focused commits and merges frequently

---

## Summary

| Concept           | Description                                                               |
| ----------------- | ------------------------------------------------------------------------- |
| Merge conflict    | When Git cannot auto-merge conflicting changes                            |
| Conflict markers  | Special text `<<<<<<<`, `=======`, `>>>>>>>` showing conflicting sections |
| Resolve conflicts | Manually edit, remove markers, stage, commit                              |
