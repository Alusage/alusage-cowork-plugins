# Odoo Helpdesk Connectors

This document explains the data connections for the Odoo Helpdesk plugin.

## Current Connectors

### Slack
- **What it's for:** Internal team communication, customer channel discussions
- **How to connect:** Available via Claude's built-in Slack MCP server

### Notion
- **What it's for:** Internal documentation, runbooks, KB articles
- **How to connect:** Available via Claude's built-in Notion MCP server

### Atlassian (Jira)
- **What it's for:** Bug tracking, feature requests, escalations
- **How to connect:** Available via Claude's built-in Atlassian MCP server

## Planned: Odoo Integration

### Option 1: JSON-RPC (Current approach)
Direct API calls to Odoo using `xmlrpc` or `requests`.

**Configuration needed:**
```json
{
  "odoo_url": "https://your-odoo.com",
  "db": "your_database",
  "username": "your_email",
  "api_key": "your_api_key"
}
```

**Models accessed:**
- `helpdesk.ticket` - Tickets
- `helpdesk.team` - Support teams
- `helpdesk.stage` - Ticket stages
- `res.partner` - Customers
- `mail.message` - Messages/responses
- `mail.activity` - Follow-ups
- `project.task` - Related tasks (if needed)

### Option 2: Custom MCP Server (Future)
A dedicated MCP server that exposes Odoo models as MCP resources and tools.

**Benefits:**
- Native Claude integration
- Type-safe
- Reusable across plugins

**Example tools:**
```
odoo_helpdesk_list_tickets
odoo_helpdesk_get_ticket
odoo_helpdesk_update_ticket
odoo_helpdesk_post_message
odoo_partner_search
odoo_partner_get
```

## Adding Your Own Connectors

Edit `.mcp.json` to add custom MCP servers:

```json
{
  "mcpServers": {
    "odoo": {
      "type": "stdio",
      "command": "python",
      "args": ["/path/to/odoo-mcp-server.py"],
      "env": {
        "ODOO_URL": "https://your-odoo.com",
        "ODOO_DB": "your_database",
        "ODOO_API_KEY": "your_api_key"
      }
    }
  }
}
```
