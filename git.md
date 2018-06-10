---
description: Git cheatsheet //in development//
---

# Git

Initialize Git
```git init```

Clone a repository
```git clone -v --progress  git@gitlab.com:USER/rep.git /var/www/html/customername```

Add a new remote pull/push destination
```git remote add origin git@gitlab.com:USER/rep.git```

Create a new branch
```git branch [branch-name] # Replace branch name```

Set branch push/push destination
```git branch --set-upstream-to origin # Or remote name created beforehand```

Stage changes to commit
```git add -A```

Stage changes of specific file
```git add [path-to-file]```

Unstage changes of specific file
```git reset HEAD [path-to-file]```

Commit changes
```git commit -m "Message"```

Add more changes to last commit
```git commit --amend```

Merge branch B to branch A
```
git checkout A
git merge B
```

