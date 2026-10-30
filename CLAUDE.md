# Video editing

## Rules

- Never watch video. Work from the WhisperX word-timestamped transcript. Screenshots only to check specific cut points.
- Cut points fall on word boundaries from the WhisperX timestamps, with 100–200 ms padding.
- Import the full cut list into Premiere as one FCP XML. For anything else touching several clips, use `add_to_timeline_batch`, `set_clip_properties_batch`, `batch_add_transitions`. Never loop over single-clip tools.
- Run `verify_premiere_connection` before editing.
- FFmpeg with `-loglevel error`.
- The main session (Opus) picks segments and reviews; subagents do the hands-on work:
  - `cutter`: audio extraction, WhisperX, FCP XML from the approved cut list.
  - `premiere-operator`: XML import, zooms, overlays, timeline checks.
  - `style-analyst`: one reference video → `_styles/style-<id>.md`.

## Projects

One folder per video, directly under `~/videos/`, named in kebab-case (`interview-maria/`, `product-demo/`):

```
~/videos/
  _styles/                  shared style reports (style-<id>.md), used by every project
  <project>/
    source/                 originals, never overwritten
    work/                   extracted audio + WhisperX transcripts
    cuts/                   approved cut lists (<name>.cutlist.json)
    xml/                    FCP XML for Premiere
    exports/                finished renders
```

- New project: `mkdir -p <project>/{source,work,cuts,xml,exports}`.
- Always launch Claude from `~/videos/` (that's where `.mcp.json`, `.venv/` and `.claude/agents/` live) and name the project at the start of the session. Every path handed to a subagent starts with `<project>/`.
- Never read or write another project's folder. Two sessions can work on two projects in parallel, but only one at a time may drive Premiere (single bridge). Name each Premiere sequence after its project.

## Setup notes

- Premiere tools are behind `search_tools` → `invoke_tool`; the names above are catalog names.
- The Premiere MCP is registered only in this folder's `.mcp.json`. Never run the premiere-pro-mcp skill's install workflow, `npm run setup:*`, or register the server in any other client.
- WhisperX lives in `.venv/` (Python 3.12, CPU, `int8`).
- Folder layout: see Projects above.
