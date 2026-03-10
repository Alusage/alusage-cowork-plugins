# Skill: Customer Research for Odoo Helpdesk

This skill provides methodology for researching customer context using Odoo data and external sources.

## Research Methodology

### 1. Understand the Question
Before searching:
- What specific information do you need?
- Why do you need it? (context for response, triage, escalation)
- What level of confidence is required?
- What's the time constraint?

### 2. Identify Relevant Sources
Based on the question type:

**Customer context:**
- Odoo partner record (res.partner)
- Sales orders and subscriptions
- Invoice and payment history
- Previous tickets

**Technical context:**
- Error logs and stack traces
- Odoo version and installed modules
- Custom code or modifications
- Integration configurations

**Product/feature information:**
- Official Odoo documentation
- Internal KB articles
- Previous resolutions to similar issues
- Known bugs list

### 3. Search Systematically
Order of sources (fastest to slowest):

1. **Quick checks** (< 1 min)
   - Odoo partner record
   - Recent tickets for same customer
   - Internal KB article search

2. **Deep dive** (1-5 min)
   - Full ticket history search
   - Log file analysis
   - Documentation review
   - Related bug reports

3. **External research** (5-15 min)
   - Odoo community forums
   - GitHub issues (if relevant module)
   - Web search for error messages
   - Stack Overflow (for code-related)

### 4. Synthesize Findings
- Combine information from multiple sources
- Note contradictions or outdated info
- Assess confidence level
- Cite sources for verification

### 5. Document and Apply
- Add findings to ticket as internal note
- Update customer record if new info discovered
- Create KB article if this will help future tickets
- Apply findings to your response

## Odoo Data Sources

### res.partner (Customer/Partner Record)
**What to look for:**
- Customer tier (tags, category)
- Contact information (email, phone)
- Company details (industry, size)
- Parent/child relationships
- Commercial fields (payment terms, credit limit)
- Internal notes

**When to use:**
- Understanding customer context
- Personalizing responses
- Escalation decisions
- SLA determination

### helpdesk.ticket (Previous Tickets)
**What to look for:**
- Similar issues reported before
- Resolution patterns
- Communication style preferences
- Response time expectations
- Escalation history

**When to use:**
- "Has this happened before?"
- Pattern recognition
- Duplicate detection
- Understanding relationship history

### sale.order (Sales Orders/Subscriptions)
**What to look for:**
- Active subscriptions
- Product licenses
- Contract start/end dates
- MRR/ARR value
- Sales rep relationship

**When to use:**
- Feature access questions
- Billing inquiries
- Upgrade/downgrade context
- Account value assessment

### account.move (Invoices)
**What to look for:**
- Payment status
- Invoice history
- Outstanding balance
- Payment terms
- Billing frequency

**When to use:**
- Billing questions
- Payment issues
- Credit hold situations
- Financial context for escalations

### mail.message (Message History)
**What to look for:**
- Past communications
- Tone and style
- Promises made
- Commitments
- Relationships with team members

**When to use:**
- Understanding communication history
- Checking what was promised
- Maintaining consistency
- Relationship context

### project.task (Related Tasks)
**What to look for:**
- Feature requests from this customer
- Custom development work
- Ongoing projects
- Implementation status

**When to use:**
- Understanding custom setup
- Feature request context
- Project dependencies
- Timeline questions

## External Research Sources

### Odoo Documentation
**Primary:** https://www.odoo.com/documentation/
- Official feature documentation
- API reference
- Configuration guides
- Video tutorials

**When to use:**
- Feature questions
- Best practices
- Configuration guidance
- API integration help

### Odoo Community Forum
**URL:** https://www.odoo.com/forum/
- Community questions and answers
- Module discussions
- Bug reports
- Workarounds

**When to use:**
- Obscure issues
- Community module questions
- Known bugs
- Creative solutions

### GitHub (Odoo/OCA)
**Odoo:** https://github.com/odoo/odoo
**OCA:** https://github.com/OCA

- Bug reports and fixes
- Feature discussions
- Code changes
- Version-specific issues

**When to use:**
- Confirmed bugs
- Version compatibility
- Code-level issues
- OCA module problems

### Internal Resources
- Confluence/Notion documentation
- Jira bug tracker
- Team Slack channels
- KB articles in Guru/Notion

**When to use:**
- Internal processes
- Known issues
- Workarounds
- Team expertise

## Research Patterns by Question Type

### "How do I...?" (Configuration/Usage)
1. Check official Odoo documentation
2. Search internal KB for how-to articles
3. Look for previous tickets with similar question
4. If not found, search community forum
5. Consider creating KB article after resolution

### "Why is it...?" (Troubleshooting)
1. Check error logs and reproduction steps
2. Search for error message in:
   - Internal bug tracker
   - Previous tickets
   - GitHub issues
   - Community forum
3. Review recent changes (upgrades, config, custom code)
4. Test in clean environment if needed

### "Can it...?" (Feature Capability)
1. Check Odoo documentation for feature scope
2. Search previous feature requests
3. Look for related modules or addons
4. Check GitHub for feature discussions
5. Consult product team if unclear

### "This customer..." (Customer Context)
1. Pull partner record
2. Review ticket history (patterns, escalations)
3. Check subscription/contract status
4. Review communication tone/style
5. Check for any special flags or notes

## Confidence Scoring

### High Confidence (90-100%)
- Information from official Odoo documentation
- Verified in test environment
- Confirmed by multiple reliable sources
- Recent (within last version)

### Medium Confidence (60-90%)
- Information from community forum (multiple confirmations)
- Internal KB article (but not recently updated)
- Single reliable source
- Applies to similar but not exact version

### Low Confidence (30-60%)
- Single community post
- Outdated information (old version)
- Partial match to question
- Extrapolation from related info

### Uncertain (<30%)
- No direct answer found
- Contradictory information
- Version mismatch
- Need to test/verify

**Always indicate confidence level** when sharing research findings!

## Research Shortcuts

### Common Questions - Quick Answers

**"What version is the customer on?"**
→ Check partner record notes or last ticket notes

**"Is this a known bug?"**
→ Search internal Jira by error message or module

**"Has this happened before?"**
→ Search tickets by customer + keyword

**"Is this feature available in their plan?"**
→ Check subscription line items

**"Who worked with this customer before?"**
→ Check ticket assignee history

## Documentation Practices

After research, document:

### In the Ticket (Internal Note)
```
Research findings:
- Partner: [name] (#[id]), [tier], [subscription status]
- Issue history: [similar tickets if any]
- Solution source: [where you found the answer]
- Confidence: [High/Medium/Low]
- Time spent: [minutes]
```

### In Customer Record (if significant)
Add internal note about:
- Special requirements or preferences
- Technical environment details
- Escalation history
- Communication preferences

### In KB (if repeatable)
If this question comes up regularly, create a KB article.

## Time Management

**Ticket Priority → Research Time Budget**
- P1: 2-5 minutes (quick check, then escalate)
- P2: 5-15 minutes (thorough but focused)
- P3: 15-30 minutes (can be thorough)
- P4: 10-20 minutes (or defer if complex)

**When to stop researching:**
- You have enough to move forward
- You're past time budget
- You need to escalate for expert help
- Customer is waiting for acknowledgment

**Better to:**
- Acknowledge quickly with "I'm researching this"
- Provide partial answer with what you know
- Escalate sooner rather than later
- Set expectations on response time

## Red Flags in Research

Watch for:
- **Contradictory information** - Different sources say different things
- **Version mismatches** - Solution is for different Odoo version
- **Custom code** - Customer has modifications affecting behavior
- **Outdated sources** - Information from old versions
- **Unofficial hacks** - Workarounds that might break things

When in doubt, ask a colleague or escalate.
