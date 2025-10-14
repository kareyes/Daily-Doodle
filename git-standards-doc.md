# Git Usage Standards

## Overview
This document establishes standardized Git practices for our team to ensure consistency, maintainability, and effective collaboration across all projects.

## Branch Naming Convention

### Main Branches
- `main` or `master` - Production-ready code
- `develop` - Integration branch for features

### Feature Branches
Format: `feature/[ticket-number]-brief-description`
- Example: `feature/JIRA-123-user-authentication`

### Bug Fix Branches
Format: `bugfix/[ticket-number]-brief-description`
- Example: `bugfix/JIRA-456-fix-login-error`

### Hotfix Branches
Format: `hotfix/[ticket-number]-brief-description`
- Example: `hotfix/JIRA-789-critical-security-patch`

### Release Branches
Format: `release/[version-number]`
- Example: `release/1.2.0`

## Commit Message Standards

### Format
```
<type>(<scope>): <subject>

<body>

<footer>
```

### Types
- `feat` - New feature
- `fix` - Bug fix
- `docs` - Documentation changes
- `style` - Code style changes (formatting, missing semicolons, etc.)
- `refactor` - Code refactoring without changing functionality
- `test` - Adding or updating tests
- `chore` - Maintenance tasks, dependency updates

### Examples
```
feat(auth): add JWT token validation

Implemented middleware to validate JWT tokens on protected routes.
Added error handling for expired and invalid tokens.

Closes JIRA-123
```

```
fix(ui): resolve button alignment issue on mobile

Adjusted flexbox properties to ensure proper button alignment
on screens smaller than 768px.

Fixes JIRA-456
```

### Rules
- Keep subject line under 50 characters
- Capitalize the subject line
- Do not end subject line with a period
- Use imperative mood ("add feature" not "added feature")
- Separate subject from body with a blank line
- Wrap body at 72 characters
- Explain what and why, not how

## Workflow

### 1. Starting New Work
```bash
git checkout develop
git pull origin develop
git checkout -b feature/JIRA-123-new-feature
```

### 2. Regular Commits
- Commit early and often
- Each commit should represent a logical unit of work
- Keep commits focused and atomic

### 3. Keeping Branch Updated
```bash
git checkout develop
git pull origin develop
git checkout feature/JIRA-123-new-feature
git rebase develop
```

### 4. Preparing for Pull Request
```bash
# Ensure all tests pass
npm test

# Check code quality
npm run lint

# Push to remote
git push origin feature/JIRA-123-new-feature
```

## Pull Request Guidelines

### Before Creating PR
- [ ] Code is tested and works locally
- [ ] All tests pass
- [ ] Code follows team style guide
- [ ] Branch is up to date with target branch
- [ ] No merge conflicts

### PR Title Format
`[TYPE] Brief description (JIRA-123)`

Example: `[FEATURE] Add user authentication (JIRA-123)`

### PR Description Template
```markdown
## Description
Brief description of changes

## Type of Change
- [ ] Bug fix
- [ ] New feature
- [ ] Breaking change
- [ ] Documentation update

## Related Ticket
JIRA-123

## Testing
Describe testing performed

## Screenshots (if applicable)
Add screenshots for UI changes

## Checklist
- [ ] Code follows style guidelines
- [ ] Self-review completed
- [ ] Comments added for complex code
- [ ] Documentation updated
- [ ] No new warnings generated
- [ ] Tests added/updated
- [ ] All tests passing
```

### Review Process
- Minimum 2 approvals required before merging
- Address all review comments
- Keep discussions professional and constructive
- Resolve conversations when addressed

## Merging Strategy

### Merge Methods
- **Squash and Merge** - For feature branches (default)
- **Rebase and Merge** - For clean linear history
- **Merge Commit** - For release branches only

### Protected Branches
- `main` and `develop` require PR and approvals
- Direct pushes to protected branches are prohibited
- CI/CD checks must pass before merging

## Best Practices

### Do's
- Write clear, descriptive commit messages
- Keep commits small and focused
- Pull latest changes frequently
- Delete branches after merging
- Tag releases appropriately
- Use `.gitignore` properly
- Review your own code before requesting review

### Don'ts
- Don't commit sensitive data (credentials, API keys)
- Don't commit generated files (unless necessary)
- Don't use `git push --force` on shared branches
- Don't commit directly to main/develop
- Don't leave commented-out code
- Don't commit work-in-progress to shared branches

## Git Configuration

### Recommended Global Settings
```bash
# Set your identity
git config --global user.name "Your Name"
git config --global user.email "your.email@company.com"

# Enable color output
git config --global color.ui auto

# Set default branch name
git config --global init.defaultBranch main

# Set pull strategy
git config --global pull.rebase true

# Enable helpful aliases
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.st status
git config --global alias.lg "log --graph --oneline --decorate --all"
```

## Troubleshooting

### Undo Last Commit (Not Pushed)
```bash
git reset --soft HEAD~1
```

### Undo Changes in Working Directory
```bash
git checkout -- <file>
```

### Resolve Merge Conflicts
```bash
# After conflict occurs
git status  # See conflicted files
# Edit files to resolve conflicts
git add <resolved-files>
git commit
```

### Stash Changes Temporarily
```bash
git stash
git stash pop  # Reapply stashed changes
```

