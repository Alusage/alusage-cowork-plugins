# Skill: Escalation Management for Odoo Helpdesk

This skill provides framework for packaging and managing escalations from support to engineering, product, or management.

## When to Escalate

### To Engineering
- Confirmed software bug
- Performance issue requiring code optimization
- Security vulnerability
- Database corruption or integrity issue
- API endpoint malfunction
- Module compatibility issue

### To Product Team
- Feature request with strong business case
- UX/UI improvement needed
- Multiple customers requesting same enhancement
- Competitive gap
- Strategic integration request

### To Management/Customer Success
- Customer threatening to churn
- Escalated complaint requiring senior attention
- SLA breach with business impact
- Contract or pricing dispute
- Relationship management needed

### To DevOps/Infrastructure
- Server or hosting issues
- Performance at infrastructure level
- Deployment or upgrade problems
- Database scaling needs
- Backup or disaster recovery issues

## Escalation Tiers

### Tier 1 → Tier 2 Support
**Criteria:**
- Technical issue beyond basic troubleshooting
- Requires log analysis or database query
- Complex configuration question
- P1 or P2 tickets

**Information needed:**
- Full reproduction steps
- What you already tried
- Odoo version and modules
- Error messages

### Tier 2 → Engineering
**Criteria:**
- Confirmed bug requiring code fix
- Performance issue requiring code change
- Security issue
- Data integrity problem

**Information needed:**
- Reproduction environment (demo/test)
- Steps to reproduce (detailed)
- Expected vs actual behavior
- Code or query causing issue (if known)
- Affected versions
- Business impact

### Support → Product
**Criteria:**
- Feature request validation
- Product direction questions
- Multiple customers need same enhancement
- Competitive feature gap

**Information needed:**
- Business use case
- Number of customers affected
- Revenue impact
- Current workarounds
- Competitive landscape

## Escalation Structure

### Standard Escalation Format

```markdown
## Escalation: [Concise Title]

**Priority:** P[1-4]
**Type:** [Bug/Feature/Performance/Security/Other]
**Escalating To:** [Team/Person]
**Odoo Ticket:** #[ticket_id]
**Jira Ticket:** [If created]

### Executive Summary
[2-3 sentences: what's wrong, why it matters, what you need]

### Business Impact
- **Severity:** [Critical/High/Medium/Low]
- **Customers Affected:** [Number and tier]
- **Revenue at Risk:** [$X MRR/ARR if applicable]
- **SLA Status:** [Breached/At Risk/Within SLA]
- **Deadline:** [If any]

### Problem Description
[Detailed description of the issue]

### Environment
- **Odoo Version:** [e.g., 17.0 Community/Enterprise]
- **Deployment:** [Odoo.sh/On-premise/Hosted]
- **Affected Module(s):** [List]
- **Database:** [Size, complexity if relevant]
- **Custom Code:** [Yes/No - describe if yes]
- **Integrations:** [Relevant integrations]

### Reproduction Steps
1. [Step 1]
2. [Step 2]
3. [Step 3]
**Expected:** [What should happen]
**Actual:** [What actually happens]

### Evidence
- Error message: `[exact error text]`
- Stack trace: [Attach or link]
- Screenshots: [Attach]
- Screen recording: [Link if available]
- Log files: [Attach or link]

### Investigation Done
- [What you've already checked]
- [What you've already tried]
- [What you've ruled out]
- [Relevant documentation consulted]

### Customer Context
- **Customer:** [Name] (#[partner_id])
- **Tier:** [Free/Standard/Enterprise]
- **Relationship:** [New/Long-term/At Risk]
- **Contract Value:** [$X MRR/ARR]
- **Key Stakeholder:** [Contact name and role]
- **History:** [Relevant background]

### Related Tickets
- Similar issue: Ticket #[X] (resolved by [solution])
- Same customer: Ticket #[Y] (related problem)
- Other customers: Ticket #[Z], #[W]

### Requested Actions
1. [Specific request #1] - [Why needed]
2. [Specific request #2] - [Why needed]
3. [Expected timeline] - [Reason for deadline]

### Customer Communication
**Current Status:** [What customer knows]
**Last Update:** [When and what was said]
**Next Update Due:** [When you promised to follow up]
**Temporary Workaround:** [If any provided]

### Proposed Next Steps
1. [Your recommendation for next action]
2. [Alternative approach if applicable]
3. [Testing plan]
4. [Communication plan]
```

## Priority Guidelines for Escalations

### P1 - Drop Everything
- Service completely down
- Data loss occurring
- Security breach active
- Financial transactions failing
- Legal/compliance violation

**Escalation SLA:** Immediate (phone/Slack)
**Engineering Response:** < 1 hour
**Updates:** Every 2 hours

### P2 - Today
- Core feature broken, no workaround
- Multiple enterprise customers affected
- SLA breach imminent
- Significant revenue impact

**Escalation SLA:** < 2 hours
**Engineering Response:** Same day
**Updates:** Daily

### P3 - This Week
- Feature impaired with workaround
- Single customer impact
- Non-critical bug
- Performance degradation (not critical)

**Escalation SLA:** < 24 hours
**Engineering Response:** < 3 days
**Updates:** Every 2-3 days

### P4 - Backlog
- Minor bug
- Feature enhancement
- Optimization opportunity
- Documentation improvement

**Escalation SLA:** When capacity allows
**Engineering Response:** Next sprint/release
**Updates:** Weekly or when prioritized

## Escalation Channels

### P1/P2
- **Jira ticket** (for tracking)
- **Slack alert** in #engineering or #escalations
- **Direct message** to on-call engineer
- **Phone call** if truly critical

### P3/P4
- **Jira ticket** (primary)
- **Slack mention** in relevant channel
- **Weekly triage meeting** (if exists)

## Quality Checklist

Before escalating, ensure you have:
- [ ] Tried basic troubleshooting
- [ ] Searched for similar issues
- [ ] Gathered all necessary information
- [ ] Tested reproduction steps
- [ ] Assessed business impact
- [ ] Checked for workarounds
- [ ] Set customer expectations
- [ ] Documented everything in ticket
- [ ] Attached relevant screenshots/logs
- [ ] Tagged with correct priority

## Common Escalation Mistakes to Avoid

### ❌ Don't
- Escalate without reproducing the issue
- Skip basic troubleshooting
- Escalate with incomplete information
- Over-escalate priority
- Leave customer waiting without acknowledgment
- Forget to set follow-up reminders
- Escalate the same issue multiple times in different tickets

### ✅ Do
- Reproduce the issue first (if possible)
- Exhaust your troubleshooting steps
- Gather complete context
- Assess priority objectively
- Keep customer informed
- Track escalation progress
- Consolidate duplicate issues

## Following Up on Escalations

### Your Responsibilities
- Monitor progress in Jira/escalation channel
- Update customer per agreed timeline
- Test fixes when provided
- Confirm resolution
- Update KB if applicable
- Close loop with customer

### When Engineering is Slow
- Check in via Slack after SLA
- Escalate to engineering manager if needed
- Keep customer updated on delays
- Seek workaround if resolution delayed
- Adjust customer expectations

### When Resolution Arrives
1. **Test the fix** in test environment
2. **Document the solution** in ticket
3. **Update customer** with clear instructions
4. **Verify with customer** that it's resolved
5. **Create KB article** if recurring issue
6. **Thank the team** who helped
7. **Close the escalation** in Jira

## Special Escalation Scenarios

### Data Loss/Corruption
- **Immediate:** Prevent further damage
- **Preserve:** Take database backup
- **Isolate:** Identify scope of impact
- **Escalate:** To senior engineering + management
- **Document:** Exact data affected
- **Communicate:** Transparently with customer

### Security Issue
- **Don't share details** publicly or in insecure channels
- **Escalate privately** to security team
- **Preserve evidence** (logs, etc.)
- **Follow security protocol** (if exists)
- **Loop in legal/compliance** if customer data exposed
- **Prepare incident report**

### Customer Escalation (Angry/Threatening to Leave)
- **De-escalate** with empathy and ownership
- **Escalate to:** Customer Success or management
- **Provide context:** Full history, relationship, value
- **Suggest:** Personal call or meeting
- **Be available:** For handoff or bridge call
- **Follow up:** After senior handles it

### Multiple Customers with Same Issue
- **Create master ticket** in Jira
- **Link all customer tickets** to master
- **Update all customers** when resolved
- **Prioritize higher** due to volume
- **Consider hotfix** if widespread
- **Post-mortem** after resolution

## Communication Templates

### Escalation Alert (Slack)
```
🚨 P[1-4] Escalation: [Brief title]
📋 Jira: [TICKET-123]
🎫 Odoo Ticket: #[ticket_id]
👤 Customer: [Name] ([Tier])
💰 Impact: [Revenue/users affected]
⏰ Deadline: [If any]

[2-sentence summary]

[Tag relevant person/team]
```

### Customer Update After Escalation
```
Hi [name],

Quick update on your issue: I've escalated this to our engineering team and it's been prioritized as [P-level]. They're reviewing it now.

Here's what happens next:
1. Engineering investigates and reproduces the issue [timeframe]
2. They develop and test a fix [timeframe]
3. Fix is deployed to your instance [timeframe]

I'll update you [frequency] on progress. In the meantime, [workaround if any].

Let me know if you have any questions.
```

### Engineering → Support Handback
When engineering provides a fix:
```
Thanks [engineer name]!

Confirming fix deployed:
- Tested in [environment]
- Verified [expected behavior]
- Validated with customer
- Resolution documented

Closing escalation. Customer ticket #[X] updated and closed.
```

## Metrics to Track

- Escalation volume by type
- Time to escalate (from ticket open)
- Time to resolve (from escalation)
- Escalation rate by support tier
- Repeat escalations (same issue)
- Customer satisfaction on escalated tickets

## Post-Escalation

After every escalation, consider:
1. **What could prevent this?** → KB article, feature change, better docs
2. **Did we escalate at right time?** → Too soon? Too late?
3. **Was information complete?** → What was missing?
4. **How can we improve?** → Process, tooling, training
5. **Should we update skills?** → Add to troubleshooting guide

Use escalations as learning opportunities to improve the whole support process.
