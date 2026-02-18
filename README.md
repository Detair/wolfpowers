# Wolfpowers

> **This is a personal testing fork. Not intended for general use.** If you want the real thing, use [superpowers](https://github.com/obra/superpowers).

A fork of [superpowers](https://github.com/obra/superpowers) by Jesse Vincent, modified to experiment with **model pinning** — assigning specific Claude models (Sonnet or Opus) to specific subagent roles. This exists solely to test whether pinning Sonnet to implementation and Opus to review improves the cost/quality tradeoff.

## What Changed From Superpowers

The only substantive change is model assignments in the three skills that dispatch subagents:

### subagent-driven-development

| Role | Model |
|---|---|
| Implementer | `sonnet` |
| Spec Compliance Reviewer | `sonnet` |
| Code Quality Reviewer | `opus` |
| Final Code Reviewer | `opus` |

### dispatching-parallel-agents

| Task Type | Model |
|---|---|
| Implementation / Fix | `sonnet` |
| Investigation / Debug | `sonnet` |
| Architecture / Analysis | `opus` |
| Review / Audit | `opus` |

### requesting-code-review

| Role | Model |
|---|---|
| Code Reviewer | `opus` |

The rationale: Sonnet is fast and cheap for focused code generation. Opus catches subtle issues that matter most during review. TDD and two-stage review already provide quality gates, so the implementer doesn't need to be the most expensive model.

### Namespace

All `superpowers:` skill references are renamed to `wolfpowers:` so the two plugins don't conflict.

## Quick Install

Paste this into Claude Code:

```
Fetch and follow instructions from https://raw.githubusercontent.com/detair/wolfpowers/refs/heads/main/.codex/INSTALL.md
```

### Alternative: Plugin Marketplace

```bash
/plugin marketplace add detair/wolfpowers-marketplace
/plugin install wolfpowers@wolfpowers-marketplace
```

### Cursor

```text
/plugin-add wolfpowers
```

### Codex

```
Fetch and follow instructions from https://raw.githubusercontent.com/detair/wolfpowers/refs/heads/main/.codex/INSTALL.md
```

### OpenCode

```
Fetch and follow instructions from https://raw.githubusercontent.com/detair/wolfpowers/refs/heads/main/.opencode/INSTALL.md
```

## How Superpowers Works

For full documentation on the workflow, skills, and philosophy, see the [superpowers README](https://github.com/obra/superpowers#readme) and [Jesse's blog post](https://blog.fsck.com/2025/10/09/superpowers/).

The short version: superpowers is a skills-based workflow system that enforces brainstorming before coding, TDD, systematic debugging, subagent-driven development with two-stage review, and git worktree isolation. Skills activate automatically — you don't invoke them manually.

## Keeping Up With Superpowers

This fork tracks superpowers upstream. To pull in new superpowers releases:

```bash
git remote add upstream https://github.com/obra/superpowers.git
git fetch upstream
git merge upstream/main
```

Then re-apply the model pinning and namespace changes if needed.

## License

MIT License - see LICENSE file for details.

## Credits

All credit for the skills system, workflow design, and philosophy goes to [Jesse Vincent](https://github.com/obra) and the superpowers contributors. This fork only adds model pinning on top.
