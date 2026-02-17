# Wolfpowers Fork Design

## Overview

Fork superpowers into wolfpowers — a fully independent Claude Code plugin with pinned AI models for specific task types. Replaces superpowers entirely.

## Goals

1. **Model pinning** — Explicit model assignments (Sonnet/Opus) for all subagent-dispatching skills
2. **Full independence** — No dependency on the superpowers plugin; all cross-references use `wolfpowers:` prefix
3. **Clean break** — Wolfpowers replaces superpowers, not coexists alongside it

## Model Assignment Strategy

Sonnet for fast focused work, Opus for deep reasoning. Applied to the 3 skills that dispatch subagents:

### subagent-driven-development

| Role | Model | Rationale |
|---|---|---|
| Implementer | `sonnet` | Fast code generation; TDD + reviews catch quality issues |
| Spec Compliance Reviewer | `sonnet` | Focused comparison task; line-by-line requirement matching |
| Code Quality Reviewer | `opus` | Deep reasoning for architecture, subtle bugs, design issues |
| Final Code Reviewer | `opus` | Holistic view across entire implementation |

### dispatching-parallel-agents

| Task Type | Model | Rationale |
|---|---|---|
| Implementation / Fix | `sonnet` | Fast, focused scope |
| Investigation / Debug | `sonnet` | Methodical tracing, Sonnet sufficient |
| Architecture / Analysis | `opus` | Deep reasoning for cross-cutting concerns |
| Review / Audit | `opus` | Holistic assessment benefits from deeper reasoning |

### requesting-code-review

| Role | Model | Rationale |
|---|---|---|
| Code Reviewer | `opus` | Deep reasoning catches subtle architecture issues |

## Changes Required

### Plugin Metadata

- `.claude-plugin/plugin.json` — Rename to "wolfpowers", update description, version 1.0.0, update author
- `.claude-plugin/marketplace.json` — Update plugin name, description, owner

### Skill Files (14 skills)

**Substantive changes (3 skills):** Add Model Assignment tables and `model:` parameter enforcement
- `skills/subagent-driven-development/SKILL.md` + prompt templates
- `skills/dispatching-parallel-agents/SKILL.md`
- `skills/requesting-code-review/SKILL.md`

**Namespace rename (all 14 skills):** Replace all `superpowers:` references with `wolfpowers:`

Cross-reference map:
- brainstorming → references writing-plans
- executing-plans → references using-git-worktrees, writing-plans, finishing-a-development-branch
- finishing-a-development-branch → references using-git-worktrees
- subagent-driven-development → references using-git-worktrees, writing-plans, requesting-code-review, finishing-a-development-branch, test-driven-development, executing-plans
- systematic-debugging → references test-driven-development, verification-before-completion
- using-git-worktrees → references finishing-a-development-branch
- using-superpowers → references brainstorming
- writing-plans → references subagent-driven-development, executing-plans
- writing-skills → references test-driven-development

### Other Files

- `agents/code-reviewer.md` — Update any superpowers: references
- `skills/requesting-code-review/code-reviewer.md` — Update references
- `skills/subagent-driven-development/implementer-prompt.md` — Update references
- `skills/subagent-driven-development/spec-reviewer-prompt.md` — Update references
- `skills/subagent-driven-development/code-quality-reviewer-prompt.md` — Update references
- `README.md` — Rebrand from superpowers to wolfpowers
- `hooks/session-start.sh` — No logic changes needed (reads from plugin root)

## Rollout

1. Apply all changes to the wolfpowers repo
2. Verify no remaining `superpowers:` references via grep
3. Install wolfpowers as local plugin, verify session-start hook loads correctly
4. Uninstall superpowers plugin
5. Delete `~/.claude/skills/wolftown-*` personal skills (now redundant)

## Future Maintenance

- Add superpowers as upstream remote: `git remote add upstream https://github.com/obra/superpowers.git`
- Diff against new superpowers releases to cherry-pick improvements
- Wolfpowers-specific features can be added freely
