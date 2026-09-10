
# Git & GitHub Cheat Sheet

## 1. `git status`

বর্তমানে আমি কোন branch-এ আছি এবং working directory-এর কী অবস্থা—তা দেখায়।

এখান থেকে বোঝা যায়:
- কোন branch-এ আছি
- কোন file modified হয়েছে
- কোন file staged হয়েছে
- কোন file untracked

```bash
git status
```

---

## 2. `git log` / `git log --oneline`

বর্তমান branch-এর commit history দেখায়।

### `git log`

প্রতিটি commit-এর বিস্তারিত তথ্য দেখায়:
- Commit ID
- Author
- Date/Time
- Commit message

```bash
git log
```

### `git log --oneline`

Commit history সংক্ষেপে এক লাইনে দেখায়।

```bash
git log --oneline
```

---

## 3. Main Branch

`main` সাধারণত repository-এর primary/default branch হিসেবে ব্যবহৃত হয়।

অন্যান্য feature/development branch সাধারণত `main` থেকে তৈরি হতে পারে এবং তাদের পরিবর্তন পরে `main`-এ merge করা হয়।

> `main`-কে সব branch-এর "top branch" বলা সব repository-র ক্ষেত্রে technically ঠিক নয়; এটি সাধারণত default/primary branch।

---

## 4. `git diff`

Staging-এ দেওয়ার আগে working directory-তে কী কী পরিবর্তন হয়েছে তা দেখায়।

```bash
git diff
```

অর্থাৎ:

```text
Working Directory
       ↓
   git diff
       ↓
Unstaged Changes
```

---

## 5. `git diff --staged`

যেসব পরিবর্তন staging area-তে রাখা হয়েছে, সেগুলোর মধ্যে commit করার আগে কী কী পরিবর্তন আছে তা দেখায়।

```bash
git diff --staged
```


> এটি remote-এ push করার আগের পরিবর্তন সরাসরি দেখায় না; মূলত staged changes review করার জন্য ব্যবহৃত হয়।

---

## 6. `git reset --hard <commitId>`

`HEAD`-কে নির্দিষ্ট commit-এ নিয়ে যায় এবং working directory ও staging area-কে সেই commit-এর অবস্থায় reset করে।

```bash
git reset --hard <commitId>
```

⚠️ সাবধান: এতে uncommitted changes হারিয়ে যেতে পারে।

---

## 7. `git reflog`

`HEAD` এবং branch reference-এর movement/history দেখায়। ভুল করে reset/rebase করলে আগের অবস্থান খুঁজে পেতে খুব useful।

```bash
git reflog
```

নির্দিষ্ট commit-এ যেতে:

```bash
git reset --hard <commitId>
```

> `git reflog <commitId>` সাধারণত valid syntax নয়। Reflog দেখতে `git reflog`, আর কোনো commit-এ ফিরে যেতে `git reset --hard <commitId>` ব্যবহার করা হয়।

---

## 8. `git config --global --list`

Global Git configuration-এর সব setting দেখায়।

```bash
git config --global --list
```

---

## 9. Git Editor Configuration

### Vim/Vi ব্যবহার করতে

```bash
git config --global core.editor "vi"
```

অথবা:

```bash
git config --global core.editor "vim"
```

### VS Code ব্যবহার করতে

```bash
git config --global core.editor "code --wait"
```

`--wait` থাকার কারণে Git, VS Code-এ কাজ শেষ না হওয়া পর্যন্ত অপেক্ষা করবে।

---

## 10. `git restore --staged <filename>`

কোনো file staging area থেকে সরিয়ে আবার unstaged অবস্থায় নিয়ে আসে।

```bash
git restore --staged <filename>
```

এতে file-এর actual changes মুছে যায় না।

```text
Staging Area
     ↓
git restore --staged
     ↓
Working Directory
```

---

## 11. `git commit --amend`

সর্বশেষ commit পরিবর্তন/আপডেট করতে ব্যবহার করা হয়।

```bash
git commit --amend
```

নতুন commit তৈরি না করে last commit-টিকেই update করা যায়।

Commit message পরিবর্তন করতেও ব্যবহার করা যায়:

```bash
git commit --amend -m "Updated commit message"
```

যদি আগের commit already remote-এ push করা থাকে, amend করার পরে history rewrite হয়। তখন সাধারণত:

```bash
git push --force-with-lease
```

ব্যবহার করা safer।

---

## 12. `git update-ref -d HEAD`

বর্তমান `HEAD` reference delete করে।

```bash
git update-ref -d HEAD
```

এটি advanced command এবং সাধারণ Git workflow-তে খুব কম ব্যবহার করা হয়।

---

## 13. Remote Repository

### Remote add

SSH দিয়ে remote add করার উদাহরণ:

```bash
git remote add origin git@github.com:USERNAME/REPOSITORY.git
```

### `git remote -v`

বর্তমান repository-এর configured remote এবং তাদের fetch/push URL দেখায়।

```bash
git remote -v
```

উদাহরণ:

```text
origin  git@github.com:USERNAME/repo.git (fetch)
origin  git@github.com:USERNAME/repo.git (push)
```

### Remote rename

```bash
git remote rename origin <givenName>
```

উদাহরণ:

```bash
git remote rename origin bongodev
```

### Remote remove

```bash
git remote remove <remoteName>
```

উদাহরণ:

```bash
git remote remove tbsr
```

---

## 14. `cd ~`

Home directory-তে চলে যায়।

```bash
cd ~
```

Windows Git Bash-এও এটি সাধারণত user's home directory-তে নিয়ে যায়।

---

## 15. `git fetch origin`

Remote repository থেকে নতুন commit/branch-এর information local repository-তে আনে, কিন্তু current branch-এর files automatically change করে না।

```bash
git fetch origin
```

সহজভাবে:

```text
Remote Repository
       ↓
   git fetch
       ↓
Remote-tracking information
```

---

## 16. `git reset --hard <remote>/<branch>`

উদাহরণ:

```bash
git reset --hard tshr/main
```

এতে current branch-এর `HEAD`, staging area এবং working directory-কে `tshr/main` যে commit-এ আছে সেই অবস্থায় নিয়ে যায়।

অর্থাৎ remote-tracking branch-এর current state-এর সাথে local state মিলিয়ে দেওয়া হয়।

 Local uncommitted changes হারিয়ে যেতে পারে।

---

## 17. `git commit`

```bash
git commit
```

`-m` না দিলে Git configured editor খুলবে এবং সেখানে commit message লিখতে হবে।

উদাহরণ:

```bash
git commit
```

অথবা সরাসরি:

```bash
git commit -m "Add new feature"
```

---

## 18. GitHub Merge Options

GitHub Pull Request merge করার সময় বিভিন্ন strategy থাকতে পারে।

### Rebase and merge

PR-এর commits-কে target branch-এর latest history-এর উপর rebase করে linear history তৈরি করে।

### Squash and merge

PR-এর একাধিক commit-কে সাধারণত একটি commit-এ combine করে target branch-এ merge করে।

```text
Multiple PR commits
        ↓
   Squash
        ↓
 Single commit
```

---

## 19. `git reset --hard tshr/main`

```bash
git reset --hard tshr/main
```

এখানে `tshr/main` হলো একটি remote-tracking branch।

এর অর্থ:

> বর্তমান local branch-কে `tshr/main` যে commit-এ আছে সেই অবস্থায় reset করা।

এটি "main-এর updated code এনে দাও" বলার কাছাকাছি, তবে আগে `fetch` করে remote information update করা ভালো:

```bash
git fetch tshr
git reset --hard tshr/main
```

---

# 20. Fork Workflow

Fork করার অর্থ হলো অন্য একটি GitHub repository-এর নিজের GitHub account-এ একটি copy তৈরি করা।

সাধারণ workflow:

### Step 1 — GitHub থেকে repository fork করা

GitHub-এর **Fork** button ব্যবহার করে repository নিজের account-এ fork করা।

### Step 2 — নিজের fork clone করা

```bash
git clone git@github.com:YOUR_USERNAME/REPOSITORY.git
```

### Step 3 — Remote check করা

```bash
git remote -v
```

সাধারণত:

```text
origin → আপনার fork
```

### Step 4 — Original repository add করা

```bash
git remote add bongodev git@github.com:bongodev/git-and-github.git
```

এখানে `bongodev` হলো remote-এর নাম।

### Step 5 — Original repository থেকে data fetch করা

```bash
git fetch bongodev
```

### Step 6 — Original main/master-এর latest state নেওয়া

```bash
git reset --hard bongodev/master
```

অথবা repository যদি `main` ব্যবহার করে:

```bash
git reset --hard bongodev/main
```

### Step 7 — নতুন branch তৈরি করা

```bash
git checkout -b my-feature
```

তারপর code পরিবর্তন করে:

```bash
git add .
git commit -m "Add feature"
git push origin my-feature
```

### Step 8 — Pull Request

নিজের fork-এর branch থেকে original/base repository-এর target branch-এ Pull Request তৈরি করা যায়।

---

# 21. Branch Rename

বর্তমান branch-এর নাম পরিবর্তন করতে:

```bash
git branch -M tshr97/git-cheat-sheet
```

যদি remote branch-ও update করতে হয়:

```bash
git push -u origin tshr97/git-cheat-sheet
```

---

# 22. Commit না করে Amend

ধরা যাক:

```text
A → B
```

`B` হলো latest commit।

আরও কিছু change করার পর নতুন commit না বানিয়ে `B`-এর মধ্যেই changes যোগ করতে:

```bash
git add .
git commit --amend
```

যদি `B` আগে remote-এ push করা হয়ে থাকে, amend করার পরে history পরিবর্তন হবে। তাই force push লাগতে পারে:

```bash
git push --force-with-lease
```

`--force`-এর চেয়ে `--force-with-lease` সাধারণত safer।

---

# 23. `git rebase bongodev/master`

বর্তমান branch-কে `bongodev/master`-এর latest history-এর উপর rebase করে।

```bash
git rebase bongodev/master
```

সাধারণ workflow:

```bash
git fetch bongodev
git rebase bongodev/master
```

---

# 24. `git rebase --continue`

Rebase-এর সময় conflict resolve করার পর:

```bash
git add <resolved-file>
git rebase --continue
```

এটি rebase process চালিয়ে যায়।

Rebase cancel করতে:

```bash
git rebase --abort
```

---

# 25. `git log -p`

Commit history-এর সাথে প্রতিটি commit-এ কী code change হয়েছে তার patch/diff দেখায়।

```bash
git log -p
```

---

# 26. `git remote remove`

কোনো remote remove করতে:

```bash
git remote remove <remoteName>
```

উদাহরণ:

```bash
git remote remove tbsr
```

---

# 27. `git checkout -t`

Remote branch থেকে local tracking branch তৈরি করতে ব্যবহার করা যায়।

```bash
git checkout -t Abdullah/add-name
```

তবে remote branch হলে সাধারণত remote name-সহ লিখতে হয়:

```bash
git checkout -t origin/Abdullah/add-name
```

এতে local branch তৈরি হবে এবং remote branch-এর সাথে tracking relationship তৈরি হবে।

---

# 28. `git stash`

বর্তমান uncommitted changes সাময়িকভাবে stash করে working directory clean করে।

```bash
git stash
```

পরে changes ফিরিয়ে আনতে:

```bash
git stash pop
```

---

# 29. `git stash list`

সব stash-এর তালিকা দেখায়।

```bash
git stash list
```

উদাহরণ:

```text
stash@{0}
stash@{1}
stash@{2}
```

নির্দিষ্ট stash apply করতে:

```bash
git stash apply stash@{0}
```

---

# 30. `git rebase -i HEAD~5`

শেষ ৫টি commit interactiveভাবে modify করার জন্য ব্যবহার করা হয়।

```bash
git rebase -i HEAD~5
```

এখানে commit:
- reorder করা যায়
- squash করা যায়
- reword করা যায়
- edit করা যায়
- drop করা যায়

Common commands:

```text
pick    → commit রাখা
reword  → commit message পরিবর্তন
edit    → commit edit করা
squash  → আগের commit-এর সাথে combine করা
fixup   → combine করা, message বাদ দেওয়া
drop    → commit বাদ দেওয়া
```

---

# Git Fetch vs Pull vs Rebase

## `git fetch`

Remote থেকে নতুন commit information আনে, কিন্তু current working files পরিবর্তন করে না।

```bash
git fetch origin
```

## `git pull`

সাধারণভাবে:

```bash
git pull
```

এটি মূলত:

```text
git fetch
+
git merge
```

অর্থাৎ remote changes fetch করার পর current branch-এর সাথে merge করে।

## `git pull --rebase`

Remote changes fetch করে current branch-এর local commits-কে remote changes-এর উপর rebase করে।

```bash
git pull --rebase
```

সাধারণভাবে history বেশি linear থাকে।

---

# Quick Git Workflow

একটি সাধারণ feature development workflow:

```bash
# Current status
git status

# Create branch
git checkout -b feature/my-feature

# Check changes
git diff

# Stage changes
git add .

# Check staged changes
git diff --staged

# Commit
git commit -m "Add my feature"

# Push
git push -u origin feature/my-feature
```

---

# Useful Recovery Workflow

যদি ভুল করে reset/rebase করা হয়:

```bash
git reflog
```

তারপর আগের commit/reference খুঁজে:

```bash
git reset --hard <commitId>
```

---

# Important Concept

Git-এর basic flow:

```text
Working Directory
       │
       │ git add
       ▼
Staging Area
       │
       │ git commit
       ▼
Local Repository
       │
       │ git push
       ▼
Remote Repository
```

আর remote থেকে update নেওয়ার ক্ষেত্রে:

```text
Remote Repository
       │
       │ git fetch
       ▼
Remote-tracking Branch
       │
       ├── git merge
       │
       └── git rebase
       ▼
Local Branch
```






1. git status
 ami kothai achi ki obostha
2. git log ,git log --oneline
 ami j branch a achi oi branch ar id ,author,timing dekha jai
3.main branch
 ata hocche top branch. ai branch theke onno sob branch dekha jabe
4.git diff 
 git staging a dawar aga akane ki ki change hoiche ta buja
5.git diff --staged 
staging theke remoite a dawar age ki ki change jacche ta fix kora
6.git reset --hard commitId
set head to the commitId
7.git reflog , git reflog commitId
show the history
8. git cofig --global --list
9.git config --global core.editor "vi"/"vim"
9.git config --global core.editor "code --wait"
10.git restore --staged filename
11.git commit --amend 
