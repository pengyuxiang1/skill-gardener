# create-grow-skill

**A meta-skill that turns skill chaos into a library — cluster similar skills behind one controller, and let experience accumulate.**

[中文](README.zh-CN.md) | **English**

> Skills pile up. Tools multiply. The same trap gets stepped on three times — because agents start every session with a brand-new memory, and skill docs never learn from your bruises.

**A grow-skill is the single runtime entry for one domain: it routes (which tool for which scenario) and it accumulates (every pit you hit, written down, never overwritten).**

Companion repo: **[skillforge](https://github.com/pengyuxiang1/skillforge)** — forges one workflow into a *super skill* (state machine + guide script + gate hook). skillforge builds one skill well; create-grow-skill governs the herd.

---

## The problem it solves

| Pain | What actually hurts |
|------|---------------------|
| Too many similar skills | Agent picks tools by guessing at `description` — which never says "when NOT to use me" |
| Experience evaporates | Lessons learned live in a chat session; session closes, gone. External skills update, your local notes get overwritten |
| Scattered installs | User dir, project dir, per-engine dirs — nobody knows the full inventory |

Existing open-source projects manage **distribution** (GUI managers, static catalogs, MCP registries). None of them manage **operating experience** — the thing that's actually worth keeping.

## The two-layer structure

```
┌ Agent engine (CodeBuddy / Claude Code / Codex / …)
│   └─ <domain>-controller/     ← inside the engine: routing table + experience log (plain Markdown)
└─ Outside the engine
    └─ hub/<skill-name>/        ← your install directory: skill bodies, version canon
```

- **hub** — wherever *you* choose (an Obsidian vault, any git repo). Skill bodies live here as the canonical copy. Engines may come and go; the hub stays.
- **controller** — one thin skill inside the engine. Two jobs only: a **scenario routing table** (rows start from "what problem does the user bring", not "what can this tool do") and an **experience log** that only ever grows.

External skill updated? It overwrites the body in the hub — the controller's experience layer is untouched. **The book gets a new edition; the librarian's notes don't burn.**

## Install

### Option 1: npx skills (recommended — 79 agents supported, CodeBuddy included)

```bash
npx skills add pengyuxiang1/create-grow-skill                       # auto-detects installed agents
npx skills add pengyuxiang1/create-grow-skill -a codebuddy -g -y    # pick agent + global + no prompts
```

### Option 2: install script

```bash
git clone https://github.com/pengyuxiang1/create-grow-skill.git
cd create-grow-skill
./install.sh                          # auto-detects installed agents
./install.sh --agent codebuddy        # or: codebuddy / claude-code / codex / cursor
./install.sh --link                   # symlink instead of copy (easy to update)
```

No clone needed:

```bash
curl -fsSL https://raw.githubusercontent.com/pengyuxiang1/create-grow-skill/main/install.sh | bash -s -- --agent codebuddy
```

### Option 3: let your agent do it

Paste this to your AI coding agent:

> Clone this repo and install the `create-grow-skill/` directory into your skills directory (CodeBuddy: `~/.codebuddy/skills/`, Claude Code: `~/.claude/skills/`).

## Quick start

1. Ask your agent: *"I have several skills for `<domain>` — help me build a unified entry (create a controller)"*
2. The skill runs **Step 0** first: it asks where your hub should live (any git-managed directory works; Obsidian users can use a folder inside the vault) and detects your engine's runtime path.
3. It inventories your domain assets, designs the scenario routing table, generates the controller, and (for aggregated skills) moves the bodies into the hub.
4. From now on, every task in that domain ends with: new pitfalls appended to the controller's `references/experience.md` — dated, evidenced, never overwritten.

## Design principles

- **Engine-agnostic** — the controller is a standard SKILL.md; anything that loads Agent Skills can use it.
- **No assumed paths** — first use runs an init guide (two questions), recorded in `<hub>/CONFIG.md`.
- **No private data baked in** — your local conventions (multi-machine path maps, your own registry) live in a git-ignored `references/config.md`; updating the skill never touches it.
- **Not designed for edge cases** — a method that's shareable can't be mixed with your own household affairs.

## License

[MIT](LICENSE)
