# Installing Wolfpowers for Codex

Enable wolfpowers skills in Codex via native skill discovery. Just clone and symlink.

## Prerequisites

- Git

## Installation

1. **Clone the wolfpowers repository:**
   ```bash
   git clone https://github.com/detair/wolfpowers.git ~/.codex/wolfpowers
   ```

2. **Create the skills symlink:**
   ```bash
   mkdir -p ~/.agents/skills
   ln -s ~/.codex/wolfpowers/skills ~/.agents/skills/wolfpowers
   ```

   **Windows (PowerShell):**
   ```powershell
   New-Item -ItemType Directory -Force -Path "$env:USERPROFILE\.agents\skills"
   cmd /c mklink /J "$env:USERPROFILE\.agents\skills\wolfpowers" "$env:USERPROFILE\.codex\wolfpowers\skills"
   ```

3. **Restart Codex** (quit and relaunch the CLI) to discover the skills.

## Migrating from old bootstrap

If you installed wolfpowers before native skill discovery, you need to:

1. **Update the repo:**
   ```bash
   cd ~/.codex/wolfpowers && git pull
   ```

2. **Create the skills symlink** (step 2 above) — this is the new discovery mechanism.

3. **Remove the old bootstrap block** from `~/.codex/AGENTS.md` — any block referencing `wolfpowers-codex bootstrap` is no longer needed.

4. **Restart Codex.**

## Verify

```bash
ls -la ~/.agents/skills/wolfpowers
```

You should see a symlink (or junction on Windows) pointing to your wolfpowers skills directory.

## Updating

```bash
cd ~/.codex/wolfpowers && git pull
```

Skills update instantly through the symlink.

## Uninstalling

```bash
rm ~/.agents/skills/wolfpowers
```

Optionally delete the clone: `rm -rf ~/.codex/wolfpowers`.
