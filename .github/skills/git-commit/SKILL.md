---
name: git-commit
description: Execute a safe commit and push workflow with explicit approval, protected-branch handling (Option C), and Conventional Commit messages.
---

# Skill: Git Commit Workflow

This skill is the operational layer for commit execution. Keep behavior safe, concise, and command-driven.

## Core Rules
- Run read-only Git commands directly to gather context.
- Execute write commands only after explicit user approval.
- Never use destructive history operations (`rebase`, `--amend`, force push, reset-based rewrites).
- Never push directly to `main`, `test`, or `dev`.
- Always detect current branch before any push command.
- Use Option C on protected branches (new branch + move only working tree changes).
- If execution is unavailable, provide runnable fallback commands in `terminal` blocks.

## Required Inputs
Ask exactly:
1. Do you confirm that you want to commit and push your changes?
2. What type of change is this? (`feat`, `fix`, `chore`, `docs`, `refactor`, `test`, `style`, `perf`)
3. Optional scope
4. Optional summary override

## Execution Flow
1. Detect branch: `git branch --show-current`.
2. If branch is protected (`main`, `test`, `dev`):
   - Check working tree: `git status --porcelain`
   - Check unpushed commits: `git log origin/<branch>..HEAD --oneline`
   - Create safe branch: `git switch -c <new-branch-name>`
3. Build one-line Conventional Commit message from diff context:
   - Prefer staged diff: `git diff --cached`
   - Else use working diff: `git diff`
4. Stage changes: `git add .`
5. Commit: `git commit -m "<type>(<scope>): <summary>"`
6. Push: `git push origin <current-branch-name>`
7. Verify clean status: `git status`

## Output Contract
- Keep explanations outside terminal blocks.
- Keep command blocks runnable and atomic.
- Warn clearly when protected-branch flow is triggered.
