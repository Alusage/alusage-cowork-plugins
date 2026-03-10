# Command: /triage

Analyze and categorize an Odoo Helpdesk ticket with priority assessment and routing recommendations.

## Usage

```
/triage <ticket description or ID>
```

## What It Does

1. **Categorizes** the issue using Odoo Helpdesk categories
2. **Assigns priority** (P1-P4) based on impact and urgency
3. **Recommends routing** to the appropriate team or stage
4. **Suggests next steps** for the support agent

## Examples

```
/triage Ticket #1234 - Customer reports 500 error on invoice generation
```

```
/triage Enterprise customer says their dashboard has been blank since this morning
```

## Output Format

```markdown
## Triage: [Brief summary]

**Ticket ID:** #1234 (if provided)
**Category:** [Technical/Billing/Feature Request/etc.]
**Priority:** P[1-4] - [Rationale]
**Product Area:** [Module/Feature]
**Customer Tier:** [Free/Standard/Enterprise]

### Routing Recommendation
Route to: [Team/Stage]
Reason: [Why this routing]

### Suggested Next Steps
1. [First action]
2. [Second action]
3. [Third action]

### Initial Response Draft
[Optional: Quick response template to acknowledge the ticket]
```

## Skills Used

- `ticket-triage/SKILL.md` - Priority framework and category taxonomy
- `customer-research/SKILL.md` - Customer context lookup

## Prerequisites

- Ticket information (can be pasted from Odoo)
- Customer context (optional, but improves accuracy)

## Tips

- Include customer tier (Free/Standard/Enterprise) for better priority assessment
- Mention SLA status if known
- Reference similar past tickets if available
