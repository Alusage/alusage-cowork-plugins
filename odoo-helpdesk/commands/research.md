# Command: /research

Research customer context and ticket history from Odoo to inform your response.

## Usage

```
/research <question or topic>
```

## What It Does

1. **Searches** Odoo partner data, ticket history, and related records
2. **Synthesizes** information from multiple sources
3. **Provides confidence score** for each finding
4. **Cites sources** (ticket IDs, partner records, etc.)

## Examples

```
/research Customer history for partner ID 123
```

```
/research Has this customer reported similar issues before?
```

```
/research What's the customer's subscription status and contract details?
```

## Output Format

```markdown
## Research: [Topic]

### Summary
[High-level answer with confidence level]

### Findings

#### Source: Odoo Partner (res.partner #123)
- **Name:** [Customer name]
- **Tier:** [Free/Standard/Enterprise]
- **Contract:** [Active/Expired/Trial]
- **Contact:** [Email, phone]

#### Source: Previous Tickets
- **Ticket #1001** (2024-01-15): [Similar issue, resolution]
- **Ticket #987** (2023-12-10): [Related topic]

#### Source: [Other sources]
[Additional context]

### Confidence: [High/Medium/Low]
[Explanation of confidence level]

### Recommended Actions
1. [First recommendation]
2. [Second recommendation]
```

## Skills Used

- `customer-research/SKILL.md` - Research methodology and source prioritization
- `ticket-triage/SKILL.md` - Pattern recognition across tickets

## Data Sources

When Odoo integration is available:
- `res.partner` - Customer details
- `helpdesk.ticket` - Ticket history
- `sale.order` - Orders and subscriptions
- `account.move` - Invoices
- `mail.message` - Communication history

Currently: Manual lookup in Odoo and paste results.

## Tips

- Be specific about what you're researching
- Mention partner ID or ticket ID if known
- Ask about patterns ("has this happened before?")
- Request specific data points (subscription, SLA, past issues)
