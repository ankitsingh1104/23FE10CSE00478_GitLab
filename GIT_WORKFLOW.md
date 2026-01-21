# Git Workflow Documentation

## Overview
This document outlines the Git workflow conventions, branching strategies, and best practices for collaborative development in this project.

## GitFlow vs GitHub Flow

### GitFlow Workflow
**Definition**: GitFlow is a robust branching model designed for planned releases with multiple environment support.

**Key Branches**:
- `main`: Production-ready code (protected branch)
- `develop`: Integration branch for features
- `feature/*`: New features from develop
- `release/*`: Release preparation from develop
- `hotfix/*`: Critical fixes from main

**Advantages**:
- Clear separation of concerns
- Multiple production versions support
- Structured release management
- Best for projects with scheduled releases

**Disadvantages**:
- Complex with many branches
- More merge conflicts
- Slower workflow for continuous deployment

### GitHub Flow
**Definition**: Simpler flow with main branch + feature branches only.

**Key Branches**:
- `main`: Always deployable production code
- `feature/*`: Short-lived feature branches

**Advantages**:
- Simple and lean
- Reduced merge conflicts
- Continuous deployment friendly
- Easy to understand

**Disadvantages**:
- Less structured for complex releases
- All changes go to main directly
- Limited multiple version support

## When to Use Rebase vs Merge

### Use MERGE when:
```bash
# Preserving full history
git merge feature-branch

# When: Merging into shared branches (develop, main)
# Result: Creates merge commit, preserves all commits
# Use case: PRs to public branches
```

### Use REBASE when:
```bash
# Creating linear history
git rebase develop

# When: Updating feature branch with latest develop
# Result: Replays your commits on top of latest code
# Use case: Keeping feature branches clean
```

### Decision Matrix:
| Scenario | Use | Reason |
|----------|-----|--------|
| Feature into main | Merge | Preserve history |
| Update feature from develop | Rebase | Linear history |
| Collaborative branch | Merge | Shared responsibility |
| Personal branch | Rebase | Clean commits |

## Commit Message Conventions

### Format
```
<type>(<scope>): <subject>

<body>

<footer>
```

### Type
- `feat`: New feature
- `fix`: Bug fix
- `docs`: Documentation changes
- `style`: Code style changes (formatting, semicolons, etc)
- `refactor`: Code refactoring without feature/fix changes
- `test`: Adding or updating tests
- `chore`: Build, dependency, or tooling changes

### Subject
- Use imperative mood: "add", "fix", "update" (not "adds", "added")
- Don't capitalize
- No period (.) at the end
- Max 50 characters

### Body (optional)
- Explain WHAT and WHY, not HOW
- Wrap at 72 characters
- Use bullet points for multiple changes

### Footer (optional)
- Reference issues: `Closes #123`
- Breaking changes: `BREAKING CHANGE: description`

### Examples
```
feat: Add user authentication module

Implement JWT-based authentication with refresh token support.
Allows users to securely login and maintain sessions.

Closes #42

fix(auth): Resolve token expiration bug

Tokens were not being refreshed properly when expired.
Updated refresh logic to check timestamp before expiry.

fix: Correct typo in config template
```

## Pull Request Template

```markdown
## Description
Brief description of changes made.

## Type of Change
- [ ] Bug fix (fixes issue #...)
- [ ] New feature (resolves #...)
- [ ] Breaking change
- [ ] Documentation update

## Related Issues
Closes #<issue_number>

## Changes Made
- Change 1
- Change 2
- Change 3

## Testing
Describe testing approach and any new tests added.

## Screenshots (if applicable)
Add screenshots for UI changes.

## Checklist
- [ ] Code follows project style guidelines
- [ ] Self-reviewed code
- [ ] Commented complex logic
- [ ] Updated documentation
- [ ] No new warnings generated
- [ ] Tests added/updated and passing
- [ ] Commits are meaningful and follow conventions
```

## Branching Strategy in Practice

### Feature Development
1. Create feature branch: `git checkout -b feature/user-auth`
2. Commit changes with proper messages
3. Push branch: `git push -u origin feature/user-auth`
4. Create pull request with template
5. Code review and approval
6. Merge to develop: `git merge feature/user-auth`

### Release Preparation
1. Create release branch: `git checkout -b release/v1.0.0`
2. Update version numbers
3. Create changelog
4. Merge to main: `git merge release/v1.0.0`
5. Tag release: `git tag -a v1.0.0 -m "Release v1.0.0"`
6. Merge back to develop

### Hotfixes
1. Create hotfix from main: `git checkout -b hotfix/critical-bug`
2. Fix issue and commit
3. Merge to main and develop
4. Tag patch release: `git tag -a v1.0.1 -m "Hotfix v1.0.1"`

## Git Commands Demonstrated

### Basic Commands
```bash
git init                    # Initialize repository
git add <file>             # Stage files
git commit -m "message"    # Commit changes
git status                 # Check status
git log --oneline          # View commit history
```

### Branching
```bash
git branch <name>          # Create branch
git checkout <branch>      # Switch branch
git switch <branch>        # Modern switch command
git branch -d <branch>     # Delete branch
git diff <branch1>..<branch2>  # Compare branches
```

### Merging & Rebasing
```bash
git merge <branch>         # Merge branch (three-way merge)
git rebase <branch>        # Rebase onto branch
git mergetool             # Resolve conflicts with tool
```

### Stashing & Cherry-picking
```bash
git stash                  # Temporarily save changes
git stash pop              # Restore stashed changes
git cherry-pick <commit>   # Apply specific commit
```

### Remote Operations
```bash
git remote add origin <url>  # Add remote
git push -u origin <branch>  # Push branch
git pull                      # Fetch and merge
git fetch                     # Fetch only
```

### Tagging
```bash
git tag -a v1.0.0 -m "Release message"  # Annotated tag
git push origin --tags                   # Push tags
```

## Best Practices

1. **Commit Frequently**: Small, logical commits are easier to review and debug
2. **Write Descriptive Messages**: Future you will thank present you
3. **Keep Branches Short-lived**: Merge within a few days to avoid conflicts
4. **Review Before Merging**: Always use pull requests for code review
5. **Protect Main Branch**: Require reviews and tests before merging
6. **Sync Regularly**: Keep feature branches updated with develop
7. **Test Before Pushing**: Run tests locally first
8. **Use .gitignore**: Prevent committing unnecessary files
9. **Delete Old Branches**: Clean up after merging
10. **Document Changes**: Keep README and docs updated

## Conflict Resolution

When conflicts occur:
```bash
# 1. See conflicting files
git status

# 2. Open file and resolve markers:
# <<<<<<< HEAD
# Your changes
# =======
# Their changes
# >>>>>>>feature-branch

# 3. Stage resolved file
git add <file>

# 4. Complete merge
git commit -m "Resolve merge conflicts"
```

## Reference
- Original GitFlow: https://nvie.com/posts/a-successful-git-branching-model/
- GitHub Flow: https://guides.github.com/introduction/flow/
- Conventional Commits: https://www.conventionalcommits.org/
