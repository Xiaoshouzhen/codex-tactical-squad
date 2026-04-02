# Install Codex Tactical Squad for Codex

[README](README.md) | [中文说明](README.zh-CN.md) | [安装说明](INSTALL.zh-CN.md)

Enable the `codex-tactical-squad` skill in Codex via native skill discovery.

## Prerequisites

- Git

## Installation

1. Clone this repository:

```bash
git clone https://github.com/<your-name>/codex-tactical-squad.git ~/.codex/codex-tactical-squad
```

2. Create the skill link.

### macOS / Linux

```bash
mkdir -p ~/.agents/skills
ln -s ~/.codex/codex-tactical-squad/skills/codex-tactical-squad ~/.agents/skills/codex-tactical-squad
```

### Windows (PowerShell)

```powershell
New-Item -ItemType Directory -Force -Path "$env:USERPROFILE\.agents\skills" | Out-Null
cmd /c mklink /J "$env:USERPROFILE\.agents\skills\codex-tactical-squad" "$env:USERPROFILE\.codex\codex-tactical-squad\skills\codex-tactical-squad"
```

3. Restart Codex.

## Verify

Confirm that the skill is visible at:

- macOS / Linux: `~/.agents/skills/codex-tactical-squad`
- Windows: `%USERPROFILE%\.agents\skills\codex-tactical-squad`

Then start a new Codex session and invoke:

```text
Use $codex-tactical-squad to handle this coding task: ...
```

## Update

```bash
cd ~/.codex/codex-tactical-squad
git pull
```

If you installed through a symlink or junction, the skill updates with the repository.

## Uninstall

Remove the skill link.

### macOS / Linux

```bash
rm ~/.agents/skills/codex-tactical-squad
```

### Windows (PowerShell)

```powershell
Remove-Item "$env:USERPROFILE\.agents\skills\codex-tactical-squad" -Recurse -Force
```

Optionally remove the cloned repository:

```bash
rm -rf ~/.codex/codex-tactical-squad
```
