# architecture-map

A Claude Code / Claude agent **skill**: map a code repo's architecture as a single self-contained HTML **C4-style container diagram** — labeled tier boxes (frontend / backend / data / external / MCP) with components nested inside and labeled arrows showing how they talk.

Every box and arrow must trace to code that actually runs — the skill reads real source (imports, call sites, config, dependency manifests), never the prose.

**Triggers:** "map the architecture", "visualize the codebase", "show me how this app is wired", "draw the stack", "make an architecture diagram".

## Install

Copy the skill into your Claude skills directory:

```bash
git clone <this-repo-url> ~/.claude/skills/architecture-map
```

Claude discovers it automatically on the next session.

## Contents

- `SKILL.md` — the skill definition
- `references/c4-style.md` — C4 diagram conventions
- `references/detection.md` — how to detect tiers/components from source
- `assets/template.html` — the HTML diagram template
