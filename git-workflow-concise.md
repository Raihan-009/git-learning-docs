# Git Workflow: Handling Behind Branch Scenarios

## Problem Statement
When working on the main branch, you've made local changes but haven't committed them. Meanwhile, other team members have pushed new commits to the remote main branch. This creates a situation where your local branch is behind the remote, and you need to integrate both sets of changes safely.

**Real Example:**
- Local changes: Added two new files (deployment.yml, ingress.yml)
- Remote status: 2 commits ahead of local
- Challenge: Cannot directly push or pull due to uncommitted changes

## Solution Approaches

### 1. Rebase Approach (Recommended for Clean History)
```bash
# 1. Stage and commit your changes
git add .
git commit -m "Add IAM configurations"

# 2. Get remote changes and rebase
git fetch origin main
git rebase origin/main

# 3. Push your changes
git push origin main
```

### 2. Stash Approach (For Work in Progress)
```bash
# 1. Stash your changes
git stash

# 2. Get and apply remote changes
git fetch origin main
git rebase origin/main

# 3. Reapply your changes
git stash pop

# 4. Commit and push
git add .
git commit -m "Add IAM configurations"
git push origin main
```

## Why Choose Rebase/Stash Over Merge?

### Rebase Benefits
- Creates linear, clean history
- Easier to understand project history
- Avoids unnecessary merge commits
- Better for feature branches that need to stay updated with main

### Stash Benefits
- Keeps work-in-progress changes safe
- Allows for clean branch switching
- Useful when changes aren't ready for commit
- Great for temporary work suspension

### Why Not Merge?
- Creates additional merge commits
- Can make history harder to follow
- More complex to revert if needed
- Can create "spaghetti history" over time

## Quick Decision Guide

Choose **Rebase** when:
- Your changes are complete and ready to commit
- You want a clean, linear history
- You're working on a feature branch

Choose **Stash** when:
- Changes are work-in-progress
- You need to switch context temporarily
- You want to test changes before committing

## Best Practices
1. Always `git fetch` before rebasing
2. Create a backup branch if unsure
3. Never rebase public/shared branches
4. Test after resolving conflicts
5. Communicate with team when handling shared branches