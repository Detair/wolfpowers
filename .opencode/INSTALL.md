# Installing Wolfpowers for OpenCode

## Prerequisites

- [OpenCode.ai](https://opencode.ai) installed
- Git installed

## Installation Steps

### 1. Clone Wolfpowers

```bash
git clone https://github.com/detair/wolfpowers.git ~/.config/opencode/wolfpowers
```

### 2. Register the Plugin

Create a symlink so OpenCode discovers the plugin:

```bash
mkdir -p ~/.config/opencode/plugins
rm -f ~/.config/opencode/plugins/wolfpowers.js
ln -s ~/.config/opencode/wolfpowers/.opencode/plugins/superpowers.js ~/.config/opencode/plugins/wolfpowers.js
```

### 3. Symlink Skills

Create a symlink so OpenCode's native skill tool discovers wolfpowers skills:

```bash
mkdir -p ~/.config/opencode/skills
rm -rf ~/.config/opencode/skills/wolfpowers
ln -s ~/.config/opencode/wolfpowers/skills ~/.config/opencode/skills/wolfpowers
```

### 4. Restart OpenCode

Restart OpenCode. The plugin will automatically inject wolfpowers context.

Verify by asking: "do you have wolfpowers?"

## Usage

### Finding Skills

Use OpenCode's native `skill` tool to list available skills:

```
use skill tool to list skills
```

### Loading a Skill

Use OpenCode's native `skill` tool to load a specific skill:

```
use skill tool to load wolfpowers/brainstorming
```

### Personal Skills

Create your own skills in `~/.config/opencode/skills/`:

```bash
mkdir -p ~/.config/opencode/skills/my-skill
```

Create `~/.config/opencode/skills/my-skill/SKILL.md`:

```markdown
---
name: my-skill
description: Use when [condition] - [what it does]
---

# My Skill

[Your skill content here]
```

### Project Skills

Create project-specific skills in `.opencode/skills/` within your project.

**Skill Priority:** Project skills > Personal skills > Wolfpowers skills

## Updating

```bash
cd ~/.config/opencode/wolfpowers
git pull
```

## Troubleshooting

### Plugin not loading

1. Check plugin symlink: `ls -l ~/.config/opencode/plugins/wolfpowers.js`
2. Check source exists: `ls ~/.config/opencode/wolfpowers/.opencode/plugins/superpowers.js`
3. Check OpenCode logs for errors

### Skills not found

1. Check skills symlink: `ls -l ~/.config/opencode/skills/wolfpowers`
2. Verify it points to: `~/.config/opencode/wolfpowers/skills`
3. Use `skill` tool to list what's discovered

### Tool mapping

When skills reference Claude Code tools:
- `TodoWrite` → `update_plan`
- `Task` with subagents → `@mention` syntax
- `Skill` tool → OpenCode's native `skill` tool
- File operations → your native tools

## Getting Help

- Report issues: https://github.com/detair/wolfpowers/issues
- Full documentation: https://github.com/detair/wolfpowers/blob/main/docs/README.opencode.md
