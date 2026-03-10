# Odoo Helpdesk Plugin

Customer support plugin for Odoo Helpdesk module. Manage tickets, draft responses, and prepare escalations using Odoo's JSON-RPC API or MCP server.

## Installation

```bash
claude plugin install odoo-helpdesk@njeudy-plugins
```

## What It Does

This plugin turns Claude into an Odoo Helpdesk co-pilot. It helps you:

- **Triage incoming tickets** from Odoo Helpdesk with structured categorization
- **Research customer questions** using Odoo partners and ticket history
- **Draft professional responses** tailored to the situation and customer context
- **Package escalations** with full context for engineering or product teams
- **Write KB articles** from resolved Odoo tickets

## Commands

| Command | Description |
|---|---|
| `/triage` | Categorize, prioritize, and route an Odoo Helpdesk ticket |
| `/research` | Research customer context from Odoo (partner, tickets, invoices) |
| `/draft-response` | Draft a customer-facing response for a ticket |
| `/escalate` | Package an escalation for engineering or product |
| `/kb-article` | Draft a knowledge base article from a resolved ticket |

## Skills

| Skill | Description |
|---|---|
| `ticket-triage` | Odoo-specific category taxonomy, priority framework, routing rules |
| `customer-research` | Research methodology using Odoo partner data and ticket history |
| `response-drafting` | Communication best practices for Odoo Helpdesk responses |
| `escalation` | Escalation format for Odoo workflows |
| `knowledge-management` | KB article structure optimized for Odoo knowledge base |

## Data Sources

### Current Setup
- Slack for internal discussions
- Notion for documentation
- Atlassian (Jira) for bug tracking

### Planned Integration
- **Odoo Helpdesk** via JSON-RPC or custom MCP server
  - `helpdesk.ticket` - Ticket management
  - `res.partner` - Customer information
  - `mail.message` - Ticket messages and responses
  - `mail.activity` - Follow-up activities

## Configuration

### Option 1: Manual (Current)
Copy-paste ticket information from Odoo when using commands.

### Option 2: JSON-RPC Script (Planned)
Configure Odoo connection in a local script that commands can call.

### Option 3: MCP Server (Future)
Full integration via a custom Odoo MCP server.

## Example Workflows

### Triaging an Odoo Ticket

```
You: /triage Ticket #1234 - Customer reports blank dashboard

Claude: Let me analyze this Odoo ticket...
[Provides triage with Odoo-specific categories and next steps]
```

### Drafting a Response

```
You: /draft-response Ticket #1234 - Need to explain the fix

Claude: [Generates professional response ready to paste in Odoo]
```

## Development Status

- [x] Base structure from customer-support
- [ ] Adapt skills for Odoo terminology
- [ ] Add Odoo JSON-RPC integration
- [ ] Create MCP server for Odoo
- [ ] Test with real Odoo Helpdesk instance

## License

MIT License
