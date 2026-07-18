# Agentic EDU — AI Agent Skills for Education

An open repository of agent skills, MCP servers, and AI agents purpose-built for education. Each skill follows the Agent Skills convention — a self-contained directory with a `SKILL.md` that declares triggers, capabilities, and execution flow.

## What's Inside

| Directory | Contains |
|---|---|
| `skills/` | Reusable AI agent skills (SKILL.md per skill) |
| `mcp-servers/` | Model Context Protocol servers for education data sources |
| `agents/` | Full agent personas and configurations |
| `docs/` | Integration guides, standards alignment |
| `examples/` | Sample outputs and workflows |

## Available Skills

### Literacy & Reading
| Skill | Description |
|---|---|
| `science-of-reading` | Evidence-based literacy skill aligned to WWC and BEE |
| `lexile-analyzer` | Analyze text readability and suggest Lexile-appropriate vocabulary |
| `hooch-reader` | Science of Reading-aligned EPUB reader with embedded xAPI |

### Standards & Curriculum
| Skill | Description |
|---|---|
| `case-framework` | Query Georgia SuitCASE and CASE competency frameworks |
| `gadoe-standards` | Georgia Standards of Excellence lookup and alignment |
| `curriculum-builder` | Generate standards-aligned lesson plans |

### Evidence & Research
| Skill | Description |
|---|---|
| `what-works-clearinghouse` | Search IES WWC for evidence-based interventions |
| `best-evidence-encyclopedia` | Search Johns Hopkins BEE for program effectiveness |

### Data & Analytics
| Skill | Description |
|---|---|
| `lrs-query` | Query xAPI Learning Record Stores for student analytics |
| `case-dashboard` | Build CASE usage analytics dashboards |

## MCP Servers

| Server | Description |
|---|---|
| `case-api` | MCP server for the Georgia CASE REST API |
| `lrs-api` | MCP server for SQL LRS xAPI queries |

## Quick Start

```bash
git clone https://github.com/agentic-edu/skills.git ~/.hermes/skills/agentic-edu/
```

## Skill Format

```
skills/science-of-reading/
├── SKILL.md
├── references/
├── scripts/
└── templates/
```

## License

MIT — use freely in classrooms, districts, and products.
