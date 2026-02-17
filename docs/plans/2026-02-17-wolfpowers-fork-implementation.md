# Wolfpowers Fork Implementation Plan

> **For Claude:** REQUIRED SUB-SKILL: Use wolfpowers:executing-plans to implement this plan task-by-task.

**Goal:** Transform the superpowers fork into wolfpowers — a fully independent plugin with pinned AI models for subagent tasks.

**Architecture:** Mechanical rename of all `superpowers:` references to `wolfpowers:`, update plugin metadata and branding, merge wolftown model-pinning content into the 3 subagent-dispatching skills.

**Tech Stack:** Markdown skills, JSON config, bash hook

---

### Task 1: Update Plugin Metadata

**Files:**
- Modify: `.claude-plugin/plugin.json`
- Modify: `.claude-plugin/marketplace.json`
- Modify: `.cursor-plugin/plugin.json`

**Step 1: Update `.claude-plugin/plugin.json`**

Change name from `"superpowers"` to `"wolfpowers"`. Update description to mention model-optimized. Set version to `"1.0.0"`. Update author to your details. Update homepage/repository URLs to `https://github.com/detair/wolfpowers`.

```json
{
  "name": "wolfpowers",
  "description": "Model-optimized skills library for Claude Code: TDD, debugging, collaboration patterns with pinned AI models per task type",
  "version": "1.0.0",
  "author": {
    "name": "detair"
  },
  "homepage": "https://github.com/detair/wolfpowers",
  "repository": "https://github.com/detair/wolfpowers",
  "license": "MIT",
  "keywords": ["skills", "tdd", "debugging", "collaboration", "best-practices", "workflows", "model-pinning"]
}
```

**Step 2: Update `.claude-plugin/marketplace.json`**

```json
{
  "name": "wolfpowers-dev",
  "description": "Development marketplace for Wolfpowers model-optimized skills library",
  "owner": {
    "name": "detair"
  },
  "plugins": [
    {
      "name": "wolfpowers",
      "description": "Model-optimized skills library for Claude Code: TDD, debugging, collaboration patterns with pinned AI models per task type",
      "version": "1.0.0",
      "source": "./",
      "author": {
        "name": "detair"
      }
    }
  ]
}
```

**Step 3: Update `.cursor-plugin/plugin.json`**

Change name to `"wolfpowers"`, displayName to `"Wolfpowers"`. Update description, version to `"1.0.0"`, author, homepage/repository URLs same as above.

**Step 4: Commit**

```bash
git add .claude-plugin/plugin.json .claude-plugin/marketplace.json .cursor-plugin/plugin.json
git commit -m "feat: rebrand plugin metadata from superpowers to wolfpowers"
```

---

### Task 2: Update Session-Start Hook

**Files:**
- Modify: `hooks/session-start.sh`

**Step 1: Update branding in session-start.sh**

Changes needed:
- Line 2 comment: `superpowers plugin` → `wolfpowers plugin`
- Line 12: `~/.config/superpowers/skills` → `~/.config/wolfpowers/skills`
- Line 14: Warning message — update `Superpowers` → `Wolfpowers`, `~/.config/superpowers/skills` → `~/.config/wolfpowers/skills`
- Line 35: `"You have superpowers."` → `"You have wolfpowers."`, `'superpowers:using-superpowers'` → `'wolfpowers:using-superpowers'`

**Step 2: Commit**

```bash
git add hooks/session-start.sh
git commit -m "feat: rebrand session-start hook from superpowers to wolfpowers"
```

---

### Task 3: Update Support Libraries

**Files:**
- Modify: `lib/skills-core.js`
- Modify: `.opencode/plugins/superpowers.js`
- Modify: `.codex/INSTALL.md`
- Modify: `docs/README.codex.md`
- Modify: `docs/README.opencode.md`

**Step 1: Update `lib/skills-core.js`**

Replace all `superpowers` references with `wolfpowers`:
- Line 58: `'superpowers'` → `'wolfpowers'` in JSDoc
- Line 101-104: Comments and JSDoc `superpowers` → `wolfpowers`
- Line 108: Function param name `superpowersDir` — keep as-is for now (internal variable), but update JSDoc
- Line 109-113: `superpowers:` prefix handling → `wolfpowers:` prefix
- Line 126-133: Variable names `superpowersPath`, `superpowersSkillFile` — rename to `wolfpowersPath`, `wolfpowersSkillFile`

**Step 2: Update `.opencode/plugins/superpowers.js`**

Replace all `superpowers` references with `wolfpowers` in this file. This includes the comment, variable names, directory paths, and string literals.

**Step 3: Update `.codex/INSTALL.md` and `docs/README.codex.md` and `docs/README.opencode.md`**

Replace all `superpowers` references with `wolfpowers`, update URLs from `obra/superpowers` to `detair/wolfpowers`.

**Step 4: Commit**

```bash
git add lib/skills-core.js .opencode/plugins/superpowers.js .codex/INSTALL.md docs/README.codex.md docs/README.opencode.md
git commit -m "feat: rebrand support libraries and platform docs to wolfpowers"
```

---

### Task 4: Add Model Pinning to subagent-driven-development

**Files:**
- Modify: `skills/subagent-driven-development/SKILL.md`
- Modify: `skills/subagent-driven-development/implementer-prompt.md`
- Modify: `skills/subagent-driven-development/spec-reviewer-prompt.md`
- Modify: `skills/subagent-driven-development/code-quality-reviewer-prompt.md`

**Step 1: Merge wolftown model-pinning into SKILL.md**

Use the content from `~/.claude/skills/wolftown-subagent-driven-development/SKILL.md` as the reference. Key additions:
- Add Model Assignment table after the "Core principle" line
- Add `[model: sonnet]` and `[model: opus]` annotations in the process flow diagram
- Add model parameters to Prompt Templates section
- Add "Cost-optimized model usage" to Advantages section
- Add "Dispatch a subagent without the specified model parameter" to Red Flags
- Update Integration section: `superpowers:` → `wolfpowers:` for all cross-references

**Step 2: Update prompt templates**

In each prompt template file, replace any `superpowers:` references with `wolfpowers:`.

**Step 3: Commit**

```bash
git add skills/subagent-driven-development/
git commit -m "feat: add model pinning to subagent-driven-development skill"
```

---

### Task 5: Add Model Pinning to dispatching-parallel-agents

**Files:**
- Modify: `skills/dispatching-parallel-agents/SKILL.md`

**Step 1: Merge wolftown model-pinning into SKILL.md**

Use content from `~/.claude/skills/wolftown-dispatching-parallel-agents/SKILL.md` as reference. Key additions:
- Add Model Assignment table with task-type-to-model mapping
- Add "Quick decision" examples
- Add model parameters to dispatch examples in code blocks
- Add "All agents on opus" to Common Mistakes
- Add "Cost efficiency" to Key Benefits
- Replace `superpowers:` refs with `wolfpowers:` (if any)

**Step 2: Commit**

```bash
git add skills/dispatching-parallel-agents/SKILL.md
git commit -m "feat: add model pinning to dispatching-parallel-agents skill"
```

---

### Task 6: Add Model Pinning to requesting-code-review

**Files:**
- Modify: `skills/requesting-code-review/SKILL.md`
- Modify: `skills/requesting-code-review/code-reviewer.md`

**Step 1: Merge wolftown model-pinning into SKILL.md**

Use content from `~/.claude/skills/wolftown-requesting-code-review/SKILL.md` as reference. Key additions:
- Add Model Assignment table (Code Reviewer → opus)
- Add `model: opus` annotations to dispatch instructions and examples
- Add "Dispatch the code reviewer with a model other than opus" to Red Flags
- Replace all `superpowers:code-reviewer` → `wolfpowers:code-reviewer`

**Step 2: Update code-reviewer.md**

Replace `superpowers:` references with `wolfpowers:`.

**Step 3: Commit**

```bash
git add skills/requesting-code-review/
git commit -m "feat: add model pinning to requesting-code-review skill"
```

---

### Task 7: Namespace Rename in Remaining Skills

**Files:**
- Modify: `skills/brainstorming/SKILL.md`
- Modify: `skills/executing-plans/SKILL.md`
- Modify: `skills/finishing-a-development-branch/SKILL.md`
- Modify: `skills/systematic-debugging/SKILL.md`
- Modify: `skills/using-git-worktrees/SKILL.md`
- Modify: `skills/using-superpowers/SKILL.md`
- Modify: `skills/writing-plans/SKILL.md`
- Modify: `skills/writing-skills/SKILL.md`
- Modify: `skills/writing-skills/testing-skills-with-subagents.md`
- Modify: `skills/test-driven-development/SKILL.md` (check for refs)
- Modify: `skills/verification-before-completion/SKILL.md` (check for refs)
- Modify: `skills/receiving-code-review/SKILL.md` (check for refs)

**Step 1: Replace `superpowers:` with `wolfpowers:` in each file**

This is a mechanical find-and-replace. For each file, replace every occurrence of `superpowers:` with `wolfpowers:`.

Also in `using-git-worktrees/SKILL.md`:
- Replace `~/.config/superpowers/worktrees` → `~/.config/wolfpowers/worktrees` (3 occurrences)

**Step 2: Commit**

```bash
git add skills/
git commit -m "feat: rename all superpowers: references to wolfpowers: across skills"
```

---

### Task 8: Update Commands and Agents

**Files:**
- Modify: `commands/brainstorm.md`
- Modify: `commands/execute-plan.md`
- Modify: `commands/write-plan.md`
- Modify: `agents/code-reviewer.md`

**Step 1: Update commands**

Replace `superpowers:` with `wolfpowers:` in each command file.

**Step 2: Update agents**

Check `agents/code-reviewer.md` for any `superpowers:` references and replace.

**Step 3: Commit**

```bash
git add commands/ agents/
git commit -m "feat: rebrand commands and agents to wolfpowers"
```

---

### Task 9: Update README and Release Notes

**Files:**
- Modify: `README.md`

**Step 1: Update README.md**

- Title: `# Wolfpowers`
- Replace all `superpowers` references with `wolfpowers` throughout
- Update installation commands to reference wolfpowers
- Update URLs from `obra/superpowers` to `detair/wolfpowers`
- Update author credit to acknowledge superpowers as upstream origin
- Add a note about model-pinning as the key differentiator

Do NOT modify `RELEASE-NOTES.md` — it's historical and should preserve superpowers history as-is.

**Step 2: Commit**

```bash
git add README.md
git commit -m "feat: rebrand README to wolfpowers with model-pinning docs"
```

---

### Task 10: Verification and Cleanup

**Step 1: Grep for remaining `superpowers:` references**

```bash
grep -r "superpowers:" --include="*.md" --include="*.json" --include="*.sh" --include="*.js" . | grep -v RELEASE-NOTES | grep -v node_modules | grep -v .git/
```

Expected: No matches (except possibly in the design doc which references the rename process, and RELEASE-NOTES.md which is historical).

**Step 2: Grep for remaining `superpowers` in config files**

```bash
grep -r '"superpowers"' --include="*.json" . | grep -v RELEASE-NOTES | grep -v .git/
```

Expected: No matches.

**Step 3: Verify hook runs**

```bash
bash hooks/session-start.sh
```

Expected: JSON output with wolfpowers branding, no errors.

**Step 4: Fix any remaining references found in Steps 1-2**

If grep finds any missed references, fix them.

**Step 5: Final commit if fixes were needed**

```bash
git add -A
git commit -m "fix: clean up remaining superpowers references"
```

---

### Task 11: Test Plugin Installation

**Step 1: Install wolfpowers as local plugin**

From Claude Code, test that the plugin loads correctly with the new branding by starting a new session.

**Step 2: Verify skills list shows `wolfpowers:` prefix**

All skills should appear as `wolfpowers:brainstorming`, `wolfpowers:subagent-driven-development`, etc.

**Step 3: Remove old wolftown personal skills**

```bash
rm -rf ~/.claude/skills/wolftown-subagent-driven-development
rm -rf ~/.claude/skills/wolftown-dispatching-parallel-agents
rm -rf ~/.claude/skills/wolftown-requesting-code-review
```
