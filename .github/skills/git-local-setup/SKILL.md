---
name: git-local-setup
description: Configure repository-local Git identity and safe defaults without modifying global configuration.
---

# Skill: Git Local Setup

This skill configures Git **locally** for this repository.

## Behavior Rules
- ALWAYS output commands inside ```terminal blocks.
- NEVER mix explanations inside terminal blocks.
- ALWAYS separate commands into safe, atomic blocks.
- NEVER use --global.
- NEVER propose rebase.
- NEVER propose push --force.
- NEVER rewrite history.
- ALWAYS detect OS (Windows/macOS) and apply correct line-ending rules.
- ALWAYS ask for confirmation before executing commands.
- ALWAYS verify the branch before suggesting pull/push.

---

## Steps

### 1. Ask the user:
- “What is your full name?”
- “What is your GitHub email?”
- “Are you on Windows or macOS?”

---

### 2. Configure identity (repo-local)
```terminal
git config user.name "<Full Name>"
```

```terminal
git config user.email "<Email>"
```

---

### 3. Configure safe pull behavior
```terminal
git config pull.rebase false
```

---

### 4. Configure explicit merge commits
```terminal
git config merge.ff false
```

---

### 5. Enable remote cleanup
```terminal
git config fetch.prune true
```

---

### 6. Improve status clarity
```terminal
git config status.showUntrackedFiles all
```

---

### 7. Configure line endings based on OS

#### If Windows:
```terminal
git config core.autocrlf true
```

#### If macOS:
```terminal
git config core.autocrlf input
```

---

### 8. Configure useful aliases
```terminal
git config alias.st "status"
```

```terminal
git config alias.co "checkout"
```

```terminal
git config alias.br "branch"
```

```terminal
git config alias.cm "commit -m"
```

```terminal
git config alias.lg "log --graph --oneline --decorate"
```

---

### 9. Verification
```terminal
git config --local --list
```

---

## Required Assistant Behavior
The assistant must:
- Guide the user step-by-step  
- Generate commands only after collecting required info  
- Always use runnable terminal blocks  
- Ensure all configuration is **local to this repo**  
- Avoid any destructive or history-altering Git operations  
