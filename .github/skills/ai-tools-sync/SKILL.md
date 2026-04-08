---
name: ai-tools-sync
description: Synchronize Cursor, Claude, and Gemini mirror files to the canonical .github prompts, skills, agents, and instructions.
---

# Universal Sync Skill (Tool-Specific Logic Included)

This skill ensures that all tools remain synchronized with the Source of Truth stored in `.github/`.

---

## 1. Source of Truth

Always use:

- `.github/copilot-instructions.md` as the entry point for all instructions
- `.github/instructions/*.instructions.md` for modular instruction files
- `.github/skills/` for canonical skills
- `.github/prompts/` for canonical prompts
- `.github/agents/` for canonical agents (`*.agent.md`)

Never duplicate content. Always reference `.github/`.

---

## 2. Tools to Synchronize

### Cursor
- Skills: `.cursor/skills/*.md`
- Prompts: `.cursor/commands/*.md`
- Agents: `.cursor/agents/*.agent.md`
- Instructions: `.cursor/rules` (single file, no extension)

### Claude Code
- Skills: `.claude/skills/*.md`
- Prompts: `.claude/prompts/*.md`
- Agents: `.claude/agents/*.agent.md`
- Instructions: `.claude/rules.md` (single file)

### Gemini Code Assist
- Skills: `.gemini/skills/*.md`
- Prompts: `.gemini/prompts/*.md`
- Agents: `.gemini/agents/*.agent.md`
- Instructions: `.gemini/rules.md` (single file)

All mirror files must contain ONLY a redirect to the corresponding `.github` file.

---

## 3. Mirror File Format (Universal)

A mirror file must contain ONLY:

```
# Mirror File
This file mirrors the canonical version stored in `.github/...`.

Please load the original file located at:
<RELATIVE PATH TO .github FILE>
```

Example:

```
Please load the original file located at:
../../.github/agents/ci-monitor-subagent.agent.md
```

---

## 4. Sync Logic

### Step 1 — Scan `.github/`
List all files in:
- `.github/skills/`
- `.github/prompts/`
- `.github/agents/`
- `.github/instructions/*.instructions.md`
- `.github/copilot-instructions.md`

### Step 2 — For each file found
For each tool:

1. Compute the correct relative path to the `.github` file.
2. Check if the mirror file exists.
3. If the mirror does not exist:
   - Create it using the correct directory and filename conventions.
4. If the mirror exists:
   - Ensure it contains ONLY the redirect.
   - If not, update it.

### Step 3 — Detect orphan files
If you find files inside `.cursor/`, `.claude/`, or `.gemini/` that do NOT exist in `.github/`:

- Do NOT modify or delete them.
- These files may be experimental, tool-specific, or pending review.
- Display this message (in French):

Ces prompts, skills ou agents ne sont pas présents dans .GitHub : [liste]. Envoie ce message à Nico pour review.

---

## 5. Instruction Files (Rules)

Each tool must have exactly ONE instruction file:

### Cursor
File: `.cursor/rules`  
Content:
```
Please follow `.github/copilot-instructions.md`.
```

### Claude
File: `.claude/rules.md`  
Content:
```
Claude, use `.github/copilot-instructions.md`.
```

### Gemini
File: `.gemini/rules.md`  
Content:
```
Gemini, follow `.github/copilot-instructions.md`.
```

---

## 6. Goal

Ensure that all tools are fully synchronized with `.github/` for all canonical instructions, skills, prompts, and agents.

This means:

- Every file in `.github/skills/`, `.github/prompts/`, `.github/agents/`, and `.github/instructions/` must have a corresponding mirror in each tool.
- Mirror files must contain ONLY a redirect to the canonical `.github` file.
- Files that exist only in tool-specific folders and not in `.github/` must be reported but never modified.

This guarantees:

- Consistency across tools  
- Maintainability through a single source of truth  
- Controlled divergence with explicit review
