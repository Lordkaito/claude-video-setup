# claude-video-setup

Claude Code setup for editing videos in Premiere Pro from WhisperX transcripts.
Working rules and folder layout are in `CLAUDE.md`.

## Contents

| Path | Purpose |
|---|---|
| `CLAUDE.md` | Editing rules and project layout |
| `.mcp.json` | Premiere Pro MCP server (`premiere-pro-mcp`) |
| `.claude/settings.json` | Enables the `premiere-pro` MCP server |
| `.claude/agents/` | Subagents: `cutter`, `premiere-operator`, `style-analyst` |
| `.claude/skills/` | 29 skills (HyperFrames family, ffmpeg-skill, premiere-pro-mcp, youtuber-style, …) |
| `skills-lock.json` | Source and hash of `premiere-pro-mcp` |
| `_styles/` | Shared style reports (`style-<id>.md`) |
| `config/settings.json` | Copy of `$CLAUDE_CONFIG_DIR/settings.json` (`~/.claude-video`) |

Video project folders and `.venv/` are ignored (see `.gitignore`).

## Setup on a new machine

```sh
git clone git@github.com:Lordkaito/claude-video-setup.git ~/videos
cd ~/videos

# WhisperX (CPU, int8) — tested with Python 3.12, whisperx 3.8.6
python3.12 -m venv .venv
.venv/bin/pip install whisperx==3.8.6

# Premiere Pro MCP server — tested with 1.2.11
npm install -g adobe-premiere-pro-mcp@1.2.11

# FFmpeg
brew install ffmpeg

# Separate Claude config dir for this setup
mkdir -p ~/.claude-video && cp config/settings.json ~/.claude-video/settings.json
CLAUDE_CONFIG_DIR=~/.claude-video claude   # always launch from ~/videos
```

Premiere also needs the MCP Bridge CEP panel; see `.claude/skills/premiere-pro-mcp/SKILL.md`.

## Third-party skills

Most skills under `.claude/skills/` are third-party (HyperFrames/HeyGen,
`ffmpeg-skill`, `hetpatel-11/Adobe_Premiere_Pro_MCP`). This repo is private;
check their licenses before making it public.
