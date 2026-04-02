# 为 Codex 安装 Codex Tactical Squad

[README](README.md) | [中文说明](README.zh-CN.md) | [Installation](INSTALL.md)

通过 Codex 的原生技能发现机制启用 `codex-tactical-squad`。

## 一句话安装

直接对 Codex 说：

```text
Fetch and follow instructions from https://raw.githubusercontent.com/Xiaoshouzhen/codex-tactical-squad/refs/heads/main/INSTALL.md
```

## 前置条件

- Git

## 安装

1. 克隆此仓库：

```bash
git clone https://github.com/Xiaoshouzhen/codex-tactical-squad.git ~/.codex/codex-tactical-squad
```

2. 创建技能链接。

### macOS / Linux

```bash
mkdir -p ~/.agents/skills
ln -s ~/.codex/codex-tactical-squad/skills/codex-tactical-squad ~/.agents/skills/codex-tactical-squad
```

### Windows（PowerShell）

```powershell
New-Item -ItemType Directory -Force -Path "$env:USERPROFILE\.agents\skills" | Out-Null
cmd /c mklink /J "$env:USERPROFILE\.agents\skills\codex-tactical-squad" "$env:USERPROFILE\.codex\codex-tactical-squad\skills\codex-tactical-squad"
```

3. 重启 Codex。

## 验证

确认技能出现在以下位置：

- macOS / Linux：`~/.agents/skills/codex-tactical-squad`
- Windows：`%USERPROFILE%\.agents\skills\codex-tactical-squad`

然后启动新的 Codex 会话并调用：

```text
使用 $codex-tactical-squad 处理这个编码任务：...
```

## 更新

```bash
cd ~/.codex/codex-tactical-squad
git pull
```

如果你通过符号链接或 junction 安装，仓库更新后技能会同步更新。

## 卸载

删除技能链接。

### macOS / Linux

```bash
rm ~/.agents/skills/codex-tactical-squad
```

### Windows（PowerShell）

```powershell
Remove-Item "$env:USERPROFILE\.agents\skills\codex-tactical-squad" -Recurse -Force
```

如果需要，也可以删除克隆下来的仓库：

```bash
rm -rf ~/.codex/codex-tactical-squad
```

### Windows（PowerShell，删除克隆仓库）

```powershell
Remove-Item "$env:USERPROFILE\.codex\codex-tactical-squad" -Recurse -Force
```
