# data.gouv.fr Skills Repository

Agent skills (using the open SKILL.md standard) for interacting with data.gouv.fr and its three APIs. Compatible with any LLM/agent supporting SKILL.md (Cursor, Claude, local models, Windsurf, etc.).

## 🧩 Available Skills

- **`datagouv-main-api`** — Catalog API: search and manage datasets, organizations, users, resources, reuses, discussions (read public; write with API key).
- **`datagouv-tabular-api`** — Query CSV/tabular rows by resource ID (from main API): filter, sort, paginate; column profile and aggregation.
- **`datagouv-metrics-api`** — Usage and download metrics by model (e.g. dataset, org): filter, sort, CSV export.
- **`datagouv-mcp`** — data.gouv.fr MCP server to automatically help and guide chatbots troughout the usa of the data.gouv.fr APIs: tool list, when to use each, and typical workflow (for when the chatbot has the MCP configured).

## ⚙️ Installation

Clone this repo, then copy the `skills/` folder (or individual skill folders) into your chatbot's skills directory:

**Cursor** (see [Cursor docs: Skills](https://cursor.com/docs/context/skills))
- **From GitHub (recommended):** Cursor Settings → Rules → Project Rules → Add Rule → *Remote Rule (Github)* → enter `https://github.com/datagouv/datagouv-skills`. Skills are then loaded from the repo without copying files.
- **Copy locally:** User-level: `cp -r skills/* ~/.cursor/skills/`. Project-level: `cp -r skills/* .cursor/skills/` (from this repo; or copy into `.cursor/skills/` of another project). Paths: [Cursor docs](https://cursor.com/docs/context/skills).

**Claude**
- MacOS: `cp -r skills/* ~/Library/Application\ Support/Claude/skills/`
- Linux: `cp -r skills/* ~/.config/claude/skills/`

**Other chatbots** — Copy `skills/*` into your client's skills directory (see its documentation for the path).

To install only some skills, copy only the folders you need (e.g. `skills/datagouv-main-api`).

## 🗂️ Repository Structure

```
skills/
├── datagouv-main-api/
├── datagouv-tabular-api/
├── datagouv-metrics-api/
└── datagouv-mcp/
    └── SKILL.md
```

## 🔌 MCP server

data.gouv.fr provides an **MCP (Model Context Protocol) server** (hosted at `https://mcp.data.gouv.fr/mcp`) so LLMs can call its APIs via tools. If your chatbot is configured with it, prefer those MCP tools when they match the operation. The **`datagouv-mcp`** skill documents the available tools, their parameters, and when to use each; the other skills (main, tabular, metrics) still give API context useful with or without MCP. Repository: https://github.com/datagouv/datagouv-mcp

## ✅ Usage

After installation, restart your LLM/agent client. Skills are automatically discovered and used when relevant.

**Cursor:** You can also invoke a skill manually by typing `/` in Agent chat and searching for the skill name. To see discovered skills: **Cursor Settings → Rules** — they appear in the **Agent Decides** section.

**Verify installation:**
```bash
# Check skills are installed
ls ~/.cursor/skills/datagouv-*          # Cursor
ls ~/Library/Application\ Support/Claude/skills/datagouv-*  # Claude (macOS)
```

## 🧠 For Local Models / Custom Clients

If you're building a custom client, you need to:
1. Discover skill subdirectories (e.g. `skills/datagouv-main-api/`, `skills/datagouv-metrics-api/`) and read the **SKILL.md** in each
2. Parse YAML frontmatter (`name`, `description`) from each file
3. Inject the relevant skill content into prompts when a task matches

**Example Python snippet:**
```python
import yaml
from pathlib import Path

def load_skills(skills_dir):
    skills = {}
    for skill_dir in Path(skills_dir).iterdir():
        skill_file = skill_dir / "SKILL.md"
        if skill_file.exists():
            content = skill_file.read_text()
            if content.startswith("---"):
                parts = content.split("---", 2)
                frontmatter = yaml.safe_load(parts[1])
                skills[frontmatter['name']] = {
                    'description': frontmatter['description'],
                    'content': parts[2].strip()
                }
    return skills
```

## 🧯 Troubleshooting

- **Skills not working?** Restart your client (skills load at startup)
- **Wrong path?** Check your client's documentation for the skills directory location

## 🤝 Contributing

When adding new skills:
1. Follow the [SKILL.md standard](https://cursor.com/docs/context/skills) (see also [agentskills.io](https://agentskills.io))
2. Keep SKILL.md under 500 lines
3. Use progressive disclosure (reference.md, examples.md for details)
4. Include clear trigger descriptions in YAML frontmatter
5. Write descriptions in third person

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
