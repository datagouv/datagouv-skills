# data.gouv.fr Skills Repository

Agent skill (SKILL.md) for interacting with data.gouv.fr and its three APIs: **Main** (catalog), **Metrics** (usage), **Tabular** (CSV rows). Compatible with any LLM/agent supporting SKILL.md (Claude Code, Cursor, ChatGPT, Codex CLI, Mistral Vibe, etc.).

## 🧩 Skill

**`SKILL.md`** — Consolidated reference for all three APIs:
- **Main API** — Datasets, organizations, users, resources, reuses, discussions, harvest, etc. (read public; write with API key)
- **Metrics API** — Usage and download metrics by model (filter, sort, CSV export)
- **Tabular API** — Query CSV/tabular rows by resource ID (filter, sort, paginate, aggregation)

## ⚙️ Installation

Clone this repo, then add the skill to your chatbot:

### 🤖 Claude Code

See [Claude Code docs: Skills](https://code.claude.com/docs/en/skills).

- Personal: `mkdir -p ~/.claude/skills/datagouv-apis && cp SKILL.md ~/.claude/skills/datagouv-apis/`
- Project-only: `mkdir -p .claude/skills/datagouv-apis && cp SKILL.md .claude/skills/datagouv-apis/`

### 🤖 Cursor

See [Cursor docs: Skills](https://cursor.com/docs/context/skills).

- **From GitHub (recommended):** Cursor Settings → Rules → Project Rules → Add Rule → *Remote Rule (Github)* → enter `https://github.com/datagouv/datagouv-skills`
- **Copy locally:** `mkdir -p ~/.cursor/skills/datagouv-apis && cp SKILL.md ~/.cursor/skills/datagouv-apis/` (or project: `.cursor/skills/datagouv-apis/`)

### 🤖 Mistral (Vibe)

See [Mistral docs: Agents & Skills](https://docs.mistral.ai/mistral-vibe/agents-skills).

- Global: `mkdir -p ~/.vibe/skills/datagouv-apis && cp SKILL.md ~/.vibe/skills/datagouv-apis/`
- Project-only: `mkdir -p .vibe/skills/datagouv-apis && cp SKILL.md .vibe/skills/datagouv-apis/`

### 🤖 Claude (desktop app)

- MacOS: `mkdir -p ~/Library/Application\ Support/Claude/skills/datagouv-apis && cp SKILL.md ~/Library/Application\ Support/Claude/skills/datagouv-apis/`
- Linux: `mkdir -p ~/.config/claude/skills/datagouv-apis && cp SKILL.md ~/.config/claude/skills/datagouv-apis/`

### 🤖 Codex CLI (OpenAI)

See [Codex docs: Skills](https://github.com/openai/codex/blob/main/docs/skills.md).

- Install: `mkdir -p ~/.codex/skills/datagouv-apis && cp SKILL.md ~/.codex/skills/datagouv-apis/`
- Run: `codex --enable skills -m <model>`

### 🤖 ChatGPT (Code Interpreter)

Paste the content of `SKILL.md` at the start of a chat, or point the model to this repo. Raw URL: `https://raw.githubusercontent.com/datagouv/datagouv-skills/main/SKILL.md`

### 🤖 Other chatbots

Copy `SKILL.md` into a folder your client treats as a skill (e.g. `datagouv-apis/SKILL.md`).

## 🗂️ Repository Structure

```
datagouv_skills/
├── SKILL.md          # Consolidated skill (Main + Metrics + Tabular APIs)
├── README.md
└── LICENSE
```

## 🔌 MCP server

data.gouv.fr provides an **[MCP (Model Context Protocol) server](https://github.com/datagouv/datagouv-mcp)** (hosted at `https://mcp.data.gouv.fr/mcp`). Use MCP tools for structured calls; use this skill for richer context and workflows. See [datagouv-mcp](https://github.com/datagouv/datagouv-mcp).

## 🧠 Usage

Restart your LLM/agent after installation. The skill is used automatically when working with data.gouv.fr.

**Cursor:** Type `/` in Agent chat and search for the skill. **Cursor Settings → Rules** shows discovered skills.

**Verify:**
```bash
ls ~/.cursor/skills/datagouv-apis/SKILL.md   # Cursor
ls ~/.claude/skills/datagouv-apis/SKILL.md   # Claude Code
```

## 🧯 Troubleshooting

- **Skill not loaded?** Restart your client
- **Wrong path?** Check your client's documentation for the skills directory

## 🤝 Contributing

1. Follow the [SKILL.md standard](https://cursor.com/docs/context/skills)
2. Keep the skill concise (token-efficient)
3. Use Swagger references for exhaustive parameter lists

## 📄 License

MIT License — see [LICENSE](LICENSE).
