# Getting Started with njeudy-plugins

Quick guide to set up and use your personal Claude Code plugins.

## Installation

### 1. Add as Local Marketplace

```bash
# Add this directory as a marketplace
claude plugin marketplace add ~/dev/IA/njeudy-plugins

# Verify it was added
claude plugin marketplace list
```

### 2. Install Plugins

```bash
# Install the Odoo Helpdesk plugin
claude plugin install odoo-helpdesk@njeudy-plugins

# Or install the generic customer-support plugin
claude plugin install customer-support@njeudy-plugins

# List installed plugins
claude plugin list
```

### 3. Verify Installation

```bash
# Start a claude code session
claude

# In the session, try a command:
/triage
```

## Using the Plugins

### Available Commands

Once installed, you have access to these slash commands:

#### `/triage <description>`
Analyze and categorize a support ticket.

**Example:**
```
/triage Customer reports 500 error when generating invoices - Enterprise tier
```

#### `/draft-response <situation>`
Generate a professional customer response.

**Example:**
```
/draft-response Need to apologize for 2-day delay on integration bug
```

#### `/research <question>`
Research customer context or technical information.

**Example:**
```
/research Has partner #1234 reported similar issues before?
```

#### `/escalate <issue>`
Package an escalation for engineering/product.

**Example:**
```
/escalate API timeout affecting 3 customers - need urgent fix
```

#### `/kb-article <topic>`
Create a knowledge base article.

**Example:**
```
/kb-article How to configure email templates in Odoo
```

## Workflow Example

Let's say you receive a new Odoo Helpdesk ticket:

```bash
# 1. Start claude code
claude

# 2. Triage the ticket
/triage Customer #1234 says dashboard is blank - started this morning

# Claude analyzes and provides:
# - Category (Bug)
# - Priority (P2 - High)
# - Routing recommendation
# - Next steps

# 3. Research the customer
/research Customer #1234 history and previous issues

# Claude searches for:
# - Partner information
# - Previous tickets
# - Pattern recognition

# 4. Draft a response
/draft-response Dashboard blank page - need to investigate logs

# Claude provides:
# - Professional response draft
# - Appropriate tone
# - Next steps for customer

# 5. If it needs escalation
/escalate Dashboard blank page - multiple customers affected

# Claude creates:
# - Structured escalation
# - Business impact assessment
# - Technical details needed
```

## Current Limitations

### Odoo Integration Not Yet Connected

The plugin is ready to use, but Odoo integration requires one of:

**Option A: Manual (Current)**
- Copy ticket details from Odoo
- Paste into Claude commands
- Copy generated responses back to Odoo

**Option B: JSON-RPC Script (Next Step)**
- Create a Python script that connects to Odoo
- Fetch ticket data programmatically
- Post responses automatically

**Option C: MCP Server (Future)**
- Build a full MCP server for Odoo
- Native integration with Claude
- Real-time data access

## Next Steps

### To Add Odoo Connection

1. **Edit `.mcp.json`** in `odoo-helpdesk/`
2. **Add Odoo MCP server** configuration (when available)
3. **Or create a helper script** for JSON-RPC calls

### To Customize

1. **Edit skills** in `odoo-helpdesk/skills/*/SKILL.md`
   - Add your specific terminology
   - Add your SLA times
   - Add your team structure
   - Add your escalation process

2. **Edit commands** in `odoo-helpdesk/commands/*.md`
   - Adjust templates
   - Add company-specific workflows
   - Modify output formats

3. **Update** plugin version in `.claude-plugin/plugin.json`

## Updating the Marketplace

After making changes:

```bash
cd ~/dev/IA/njeudy-plugins
git add .
git commit -m "[IMP] your description"

# Then update the marketplace
claude plugin marketplace update njeudy-plugins
```

## Pushing to GitHub (Later)

When ready to share or backup to GitHub:

```bash
# Create repo on GitHub first, then:
cd ~/dev/IA/njeudy-plugins
git remote add origin git@github.com:njeudy/claude-plugins.git
git push -u origin main

# Then update marketplace to use GitHub instead of local path
claude plugin marketplace remove njeudy-plugins
claude plugin marketplace add njeudy/claude-plugins
```

## Troubleshooting

### Plugin not found
```bash
# Check marketplace is added
claude plugin marketplace list

# Check plugins in marketplace
claude plugin search helpdesk@njeudy-plugins
```

### Command not working
```bash
# Reinstall the plugin
claude plugin uninstall odoo-helpdesk
claude plugin install odoo-helpdesk@njeudy-plugins
```

### Changes not showing
```bash
# Update the marketplace
claude plugin marketplace update njeudy-plugins

# Or reload plugins
claude plugin reload
```

## Getting Help

- Check plugin README: `~/dev/IA/njeudy-plugins/odoo-helpdesk/README.md`
- Check command docs: `~/dev/IA/njeudy-plugins/odoo-helpdesk/commands/*.md`
- Check skills: `~/dev/IA/njeudy-plugins/odoo-helpdesk/skills/*/SKILL.md`

## Resources

- [Claude Code Documentation](https://docs.anthropic.com/claude-code)
- [Plugin Development Guide](https://github.com/anthropics/knowledge-work-plugins)
- [MCP Protocol](https://modelcontextprotocol.io/)
- [Odoo API Documentation](https://www.odoo.com/documentation/17.0/developer/reference/external_api.html)
