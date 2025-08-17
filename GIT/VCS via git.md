# Git utility - Version Contril Service(VCS)

```bash
git config --global user.name ""

git config --global user.email ""
```

```bash
git init

git remote -v

git remote set-url origin https://github.com/directory.git


git diff

git log

git status

git log -p -5

git log --pretty=oneline

git log --pretty=format:"%h - %an , %ar:%S

git log -S "String"

git log --grep="string"

git log --since="2024-10-10"

git log --until="2024-10-10"

git log --author="paul"
```

```bash
git show <commit_id>:file

git checkout <commit_id>

git checkout master
```

```bash

# Negative use case: 1
git restore .

git restore file

#Negative use case: 2

git restore --staged .

git restore --staged <file>

git restore <file> or .

# Negative use case: 3

git restore --worktree <file> or .

then can use:
git restore --staged <file> or .

git restore . or <file>

# Negative use case: 4

git reset --hard HEAD^

git reset --soft HEAD^
```

```bash

git pull

git clone
```

```bash
git branch design

git checkout design

git checkout main

git merge design

git push origin main
```

```bash
# Merge Conflict
```

```bash
# Git Forking
```

```bash
# .gitignore --> hide the file from public

# Git clean

# Git Tag
```