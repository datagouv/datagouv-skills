# data.gouv.fr Skills Repository

Agent skills (using the open SKILL.md standard) for interacting with data.gouv.fr and its three APIs. Compatible with any LLM/agent supporting SKILL.md (Cursor, Claude, local models, Windsurf, etc.).

## Available Skills

- **`datagouv-main-api`** - Main API for datasets, organizations, users, and resources
- **`datagouv-tabular-api`** - Tabular data browsing and querying
- **`datagouv-metrics-api`** - Usage statistics and analytics
- **`datagouv-mcp`** - data.gouv.fr MCP server: tool list, when to use each, and typical workflow (for when the chatbot has the MCP configured)

## Installation

Clone this repo, then copy the `skills/` folder (or individual skill folders) into your chatbot's skills directory:

**Cursor**
- Personal: `cp -r skills/* ~/.cursor/skills/`
- Project: `cp -r skills/* .cursor/skills/`

**Claude**
- MacOS: `cp -r skills/* ~/Library/Application\ Support/Claude/skills/`
- Linux: `cp -r skills/* ~/.config/claude/skills/`

**Other chatbots** — Copy `skills/*` into your client's skills directory (see its documentation for the path).

To install only some skills, copy only the folders you need (e.g. `skills/datagouv-main-api`).

## Repository Structure

```
skills/
├── datagouv-main-api/
├── datagouv-tabular-api/
├── datagouv-metrics-api/
└── datagouv-mcp/          # MCP server tools (when MCP is configured)
    └── SKILL.md
```

## MCP server

data.gouv.fr provides an **MCP (Model Context Protocol) server** (hosted at `https://mcp.data.gouv.fr/mcp`) so LLMs can call its APIs via tools. If your chatbot is configured with it, prefer those MCP tools when they match the operation. The **`datagouv-mcp`** skill documents the available tools, their parameters, and when to use each; the other skills (main, tabular, metrics) still give API context useful with or without MCP.

## Usage

After installation, restart your LLM/agent client. Skills are automatically discovered and used when relevant.

**Verify installation:**
```bash
# Check skills are installed
ls ~/.cursor/skills/datagouv-*          # Cursor
ls ~/Library/Application\ Support/Claude/skills/datagouv-*  # Claude (macOS)
```

## For Local Models / Custom Clients

If you're building a custom client, you need to:
1. Read SKILL.md files from the skills directory
2. Parse YAML frontmatter (`name`, `description`)
3. Inject skill content into prompts when relevant

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

## Troubleshooting

- **Skills not working?** Restart your client (skills load at startup)
- **Wrong path?** Check your client's documentation for the skills directory location

## Contributing

When adding new skills:
1. Follow the [SKILL.md standard](https://github.com/getcursor/skills)
2. Keep SKILL.md under 500 lines
3. Use progressive disclosure (reference.md, examples.md for details)
4. Include clear trigger descriptions in YAML frontmatter
5. Write descriptions in third person

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
