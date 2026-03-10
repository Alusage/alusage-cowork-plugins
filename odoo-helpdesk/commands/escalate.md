# Command: /escalate

Package a structured escalation for engineering, product, or management teams.

## Usage

```
/escalate <issue description>
```

## What It Does

1. **Structures escalation** with all necessary context
2. **Assesses business impact** and urgency
3. **Provides reproduction steps** if technical
4. **Recommends escalation tier** and assignee
5. **Sets follow-up expectations**

## Examples

```
/escalate API returning 500 errors - 3 Enterprise customers affected this week
```

```
/escalate Customer requesting feature that's on roadmap - need product input
```

```
/escalate Data integrity issue - customer seeing wrong invoice totals
```

## Output Format

```markdown
## Escalation: [Brief title]

### Summary
[2-3 sentence overview of the issue]

### Business Impact
- **Severity:** [Critical/High/Medium/Low]
- **Affected Customers:** [Number and tier]
- **Revenue Impact:** [If applicable]
- **SLA Status:** [Breached/At Risk/Within SLA]

### Technical Details

**Issue:**
[Detailed description]

**Reproduction Steps:**
1. [Step 1]
2. [Step 2]
3. [Expected vs actual result]

**Environment:**
- Odoo Version: [version]
- Module(s): [affected modules]
- Custom Code: [Yes/No, details]

### Customer Context
- **Partner:** [Name, ID]
- **Tier:** [Free/Standard/Enterprise]
- **History:** [Relevant background]

### Escalation Routing
- **Team:** [Engineering/Product/Management]
- **Priority:** P[1-4]
- **Assignee:** [Suggested person if known]

### Requested Actions
1. [What you need from the escalation team]
2. [Expected timeline]
3. [Follow-up plan]

### Customer Communication
- **Current Status:** [What customer knows]
- **Next Update Due:** [When to follow up]
- **Suggested Message:** [Draft for next customer update]

---

**Related Tickets:** [List ticket IDs]
**Odoo Ticket:** #[ticket_id]
```

## Skills Used

- `escalation/SKILL.md` - Escalation framework and impact assessment
- `ticket-triage/SKILL.md` - Priority and routing logic

## Escalation Tiers

- **P1 (Critical):** Service down, data loss, security breach
- **P2 (High):** Core feature broken, multiple customers affected
- **P3 (Medium):** Feature impaired, workaround available
- **P4 (Low):** Minor issue, single customer, cosmetic

## Tips

- Include specific error messages or logs
- Mention if issue is reproducible in demo/test environment
- Note any temporary workarounds provided to customer
- Attach screenshots or recordings if available
- Reference related tickets or known bugs
