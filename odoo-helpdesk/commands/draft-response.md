# Command: /draft-response

Generate a professional, empathetic response for an Odoo Helpdesk ticket.

## Usage

```
/draft-response <situation description>
```

## What It Does

1. **Analyzes** the situation and customer sentiment
2. **Selects appropriate tone** (empathetic, technical, apologetic, etc.)
3. **Drafts response** following Odoo Helpdesk best practices
4. **Includes next steps** and sets expectations
5. **Formats** for direct copy-paste into Odoo message composer

## Examples

```
/draft-response Customer frustrated by 2-day delay on integration issue - need to take ownership
```

```
/draft-response Simple how-to question about configuring email templates
```

```
/draft-response Bug confirmed, engineering working on fix, ETA 48h
```

## Output Format

```markdown
## Response Draft

**Tone:** [Empathetic/Professional/Technical/Apologetic]
**Type:** [Resolution/Update/Question/Escalation]

---

[Draft message ready to copy-paste]

---

### Internal Notes
- [Suggested internal action]
- [Follow-up reminder]
- [Related documentation to update]
```

## Skills Used

- `response-drafting/SKILL.md` - Communication templates and tone guidelines
- `customer-research/SKILL.md` - Customer context for personalization

## Tone Guidelines

- **P1 Incidents:** Empathetic, ownership, clear timeline
- **Feature Requests:** Appreciative, explain process
- **How-to Questions:** Helpful, educational
- **Bug Reports:** Apologetic if impacting work, clear on next steps
- **Complaints:** Empathetic, de-escalation, ownership

## Tips

- Mention ticket ID for internal tracking
- Include customer name if known (Claude can look it up)
- Specify if response needs executive review
- Note if this should trigger a follow-up activity
