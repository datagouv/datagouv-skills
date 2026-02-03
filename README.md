# data.gouv.fr Skills Repository

Agent skills (using the open SKILL.md standard) for interacting with data.gouv.fr and its three APIs. Compatible with any LLM/agent supporting SKILL.md (Claude Code, Cursor, ChatGPT, Codex CLI, Mistral Vibe, etc.).

## 🧩 Available Skills

- **`datagouv-main-api`** — Catalog API: search and manage datasets, organizations, users, resources, reuses, discussions (read public; write with API key).
- **`datagouv-tabular-api`** — Query CSV/tabular rows by resource ID (from main API): filter, sort, paginate; column profile and aggregation.
- **`datagouv-metrics-api`** — Usage and download metrics by model (e.g. dataset, org): filter, sort, CSV export.

## ⚙️ Installation

Clone this repo, then copy the `skills/` folder (or individual skill folders) into your chatbot's skills directory:

### 🤖 Claude Code

See [Claude Code docs: Skills](https://code.claude.com/docs/en/skills).

- Personal (all projects): `cp -r skills/* ~/.claude/skills/`
- Project-only: `cp -r skills/* .claude/skills/` (from this repo into `.claude/skills/` of the target project)

### 🤖 Cursor

See [Cursor docs: Skills](https://cursor.com/docs/context/skills).

- **From GitHub (recommended):** Cursor Settings → Rules → Project Rules → Add Rule → *Remote Rule (Github)* → enter `https://github.com/datagouv/datagouv-skills`. Skills are then loaded from the repo without copying files.
- **Copy locally:** User-level: `cp -r skills/* ~/.cursor/skills/`. Project-level: `cp -r skills/* .cursor/skills/` (from this repo; or copy into `.cursor/skills/` of another project). Paths: [Cursor docs](https://cursor.com/docs/context/skills).

### 🤖 Mistral (Vibe)

See [Mistral docs: Agents & Skills](https://docs.mistral.ai/mistral-vibe/agents-skills).

Vibe discovers skills from directories containing `SKILL.md` (same format: YAML frontmatter + markdown). Custom paths and enable/disable patterns: `config.toml` (`skill_paths`, `enabled_skills`, `disabled_skills`).

- Global (all projects): `cp -r skills/* ~/.vibe/skills/`
- Project-only: `cp -r skills/* .vibe/skills/` (from this repo into `.vibe/skills/` of the target project)

### 🤖 Claude (desktop app)

- MacOS: `cp -r skills/* ~/Library/Application\ Support/Claude/skills/`
- Linux: `cp -r skills/* ~/.config/claude/skills/`

### 🤖 Codex CLI (OpenAI)

See [Codex docs: Skills](https://github.com/openai/codex/blob/main/docs/skills.md).

Experimental support. Any folder under `~/.codex/skills` is treated as a skill (must contain `SKILL.md`).

- Install: `cp -r skills/* ~/.codex/skills/` (or clone this repo into `~/.codex/skills/datagouv-skills` and symlink/copy the skill subfolders).
- Run with skills enabled: `codex --enable skills -m <model>`. In the session, type `list skills` to see loaded skills.

### 🤖 ChatGPT (Code Interpreter)

ChatGPT’s Code Interpreter uses a similar skill format: a `/home/oai/skills` folder with subfolders each containing a `skill.md`. That folder is part of the sandbox and is not user-installable (it is pre-populated by OpenAI with e.g. PDF/docs/spreadsheet skills). To use data.gouv.fr skills in ChatGPT:
- **Per conversation:** At the start of a chat, paste the content of the relevant `SKILL.md` (and optionally `reference.md`) from this repo, or upload those files, and ask the model to follow that guidance when working with the data.gouv.fr API.
- **Reference:** You can point the model to this repo (e.g. “Use the data.gouv.fr API as described in the [datagouv-skills repo](https://github.com/datagouv/datagouv-skills)”) and share the raw file URLs (e.g. from GitHub) so it can fetch and apply the instructions.

### 🤖 Other chatbots

Copy `skills/*` into your client's skills directory (see its documentation for the path).

To install only some skills, copy only the folders you need (e.g. `skills/datagouv-main-api`).

## 🗂️ Repository Structure

```
skills/
├── datagouv-main-api/
│   ├── SKILL.md
│   ├── reference.md
│   └── examples.md
├── datagouv-tabular-api/
│   ├── SKILL.md
│   └── reference.md
└── datagouv-metrics-api/
    ├── SKILL.md
    └── reference.md
```

## 🔌 MCP server

data.gouv.fr provides an **[MCP (Model Context Protocol) server](https://github.com/datagouv/datagouv-mcp)** (hosted at `https://mcp.data.gouv.fr/mcp`) so LLMs can call its APIs via tools. Both options are valid: **MCP tools** offer structured calls with clear parameters; **skills** provide richer context (workflows, examples, edge cases) and can sometimes yield better results on complex tasks. Use whichever fits your setup — or both. See the [datagouv-mcp repository](https://github.com/datagouv/datagouv-mcp) for MCP setup and documentation.

## 🧠 Usage

After installation, restart your LLM/agent client. Skills are automatically discovered and used when relevant.

**Cursor:** You can also invoke a skill manually by typing `/` in Agent chat and searching for the skill name. To see discovered skills: **Cursor Settings → Rules** — they appear in the **Agent Decides** section.

**Verify installation:**
```bash
# Check skills are installed
ls ~/.claude/skills/datagouv-*          # Claude Code (personal)
ls ~/.cursor/skills/datagouv-*          # Cursor
ls ~/.codex/skills/datagouv-*           # Codex CLI
ls ~/.vibe/skills/datagouv-*            # Mistral Vibe (global)
ls ~/Library/Application\ Support/Claude/skills/datagouv-*  # Claude desktop (macOS)
```

## 🧯 Troubleshooting

- **Skills not working?** Restart your client (skills load at startup)
- **Wrong path?** Check your client's documentation for the skills directory location

## 🤝 Contributing

When adding new skills:
1. Follow the [SKILL.md standard](https://cursor.com/docs/context/skills) (see also [agentskills.io](https://agentskills.io))
2. Keep SKILL.md under 500 lines
3. Use progressive disclosure (reference.md, examples.md for details)
4. Write descriptions in third person

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
