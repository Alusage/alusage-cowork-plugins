# njeudy-plugins

Personal Claude Code plugins for professional workflows.

## Available Plugins

### customer-support
Triage tickets, draft responses, escalate issues, and build knowledge base. Based on Anthropic's knowledge-work-plugins.

**Source:** Imported from [anthropics/knowledge-work-plugins](https://github.com/anthropics/knowledge-work-plugins)

### odoo-helpdesk
Customer support plugin specifically adapted for Odoo Helpdesk module. Manage tickets, draft responses, and prepare escalations using Odoo's JSON-RPC API.

**Status:** In development

## Installation

### As Local Marketplace

```bash
# Add this repository as a local marketplace
claude plugin marketplace add ~/dev/IA/njeudy-plugins

# Install a plugin
claude plugin install customer-support@njeudy-plugins
claude plugin install odoo-helpdesk@njeudy-plugins
```

### As GitHub Marketplace (after push)

```bash
# Add from GitHub
claude plugin marketplace add njeudy/claude-plugins

# Install plugins
claude plugin install customer-support@njeudy/claude-plugins
```

## Structure

```
njeudy-plugins/
├── customer-support/       # Generic customer support (base)
│   ├── .claude-plugin/
│   ├── commands/
│   ├── skills/
│   └── .mcp.json
├── odoo-helpdesk/         # Odoo-specific adaptation
│   ├── .claude-plugin/
│   ├── commands/
│   ├── skills/
│   └── .mcp.json
└── README.md
```

## Development

Each plugin follows the Claude Code plugin structure:
- `.claude-plugin/plugin.json` - Plugin manifest
- `commands/` - Slash commands (/triage, /draft-response, etc.)
- `skills/` - Domain knowledge and workflows
- `.mcp.json` - MCP server connections (optional)

## License

MIT License - See LICENSE file in each plugin directory
