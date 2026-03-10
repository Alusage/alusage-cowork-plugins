# Skill: Ticket Triage for Odoo Helpdesk

This skill provides the framework for categorizing, prioritizing, and routing Odoo Helpdesk tickets.

## Priority Framework

### P1 - Critical (Immediate response required)
- **Service Down:** Complete system outage or critical module unavailable
- **Data Loss/Corruption:** Risk of or actual data loss
- **Security Issue:** Active security vulnerability or breach
- **Revenue Impact:** Direct impact on customer's revenue generation
- **SLA:** Enterprise customers with breached SLA

**Response SLA:** < 1 hour
**Resolution Target:** < 4 hours

### P2 - High (Same day response)
- **Core Feature Broken:** Important feature not working, no workaround
- **Multiple Customers Affected:** Same issue reported by 3+ customers
- **Enterprise Impact:** Enterprise customer with significant business impact
- **Payment/Billing Issues:** Blocking customer payments or transactions

**Response SLA:** < 4 hours
**Resolution Target:** < 24 hours

### P3 - Medium (Next business day)
- **Feature Impaired:** Feature works but with limitations
- **Workaround Available:** Issue exists but can be worked around
- **Single Customer Impact:** Affecting one customer's workflow
- **Configuration Issues:** Misconfiguration causing problems

**Response SLA:** < 24 hours
**Resolution Target:** < 72 hours

### P4 - Low (Best effort)
- **Feature Request:** New functionality or enhancement
- **Cosmetic Issue:** Visual bug with no functional impact
- **Question:** How-to or documentation request
- **Minor Inconvenience:** Small usability issue

**Response SLA:** < 48 hours
**Resolution Target:** As capacity allows

## Category Taxonomy

### Technical Issues
- **Bug:** Confirmed software defect
- **Performance:** Slow loading, timeouts, resource issues
- **Integration:** Third-party service connection problems
- **Database:** Data integrity, migration, or query issues
- **API:** REST/JSON-RPC endpoint problems

### Configuration
- **Setup:** Initial configuration or installation
- **Permissions:** Access rights and user roles
- **Workflow:** Business process configuration
- **Email:** Email server, templates, automation
- **Reports:** Report generation or customization

### Billing & Account
- **Invoice:** Invoicing issues or questions
- **Payment:** Payment processing or methods
- **Subscription:** Plan changes, renewals, cancellations
- **Pricing:** Pricing questions or discrepancies

### Feature Requests
- **Enhancement:** Improvement to existing feature
- **New Feature:** Completely new functionality
- **Integration Request:** Request for new third-party integration

### How-To / Questions
- **Documentation:** Question answerable from documentation
- **Training:** User needs guidance on how to use feature
- **Best Practice:** Seeking advice on optimal usage

### Other
- **Data Request:** Export, import, or data manipulation
- **Customization:** Custom development request
- **Consultation:** Strategic or architectural guidance

## Routing Rules

### Tier 1 Support (First Response)
- Initial triage and categorization
- Answer simple how-to questions
- Perform basic troubleshooting
- Escalate if needed

**Handle:**
- P3 and P4 tickets
- How-to questions
- Configuration guidance
- Known issues with documented solutions

**Escalate to Tier 2:**
- Technical issues requiring investigation
- P1 and P2 tickets
- Unknown errors or bugs
- Custom code issues

### Tier 2 Support (Advanced)
- Technical investigation and debugging
- Database analysis
- Log file review
- Complex configuration issues

**Handle:**
- Most technical issues
- Performance problems
- Integration troubleshooting
- Complex workflows

**Escalate to Engineering:**
- Confirmed bugs requiring code changes
- Security vulnerabilities
- Database design issues
- New feature requests for evaluation

### Engineering
- Code fixes and patches
- New feature development
- Security patches
- Architecture changes

**Handle:**
- Confirmed software bugs
- Security issues
- Feature development
- Database schema changes

### Product Team
- Feature request evaluation
- Roadmap questions
- Strategic integrations
- UX/UI improvements

**Handle:**
- Feature requests with business case
- Product direction questions
- Integration priorities
- Design feedback

## Odoo-Specific Considerations

### Common Modules
- **Sales:** CRM, quotations, sales orders
- **Accounting:** Invoicing, payments, reconciliation
- **Inventory:** Stock, warehouses, deliveries
- **Manufacturing:** MRP, work orders, BOMs
- **Website:** eCommerce, portal, CMS
- **Helpdesk:** This module itself
- **Project:** Tasks, timesheets, planning
- **HR:** Employees, attendance, expenses

### Version Considerations
- Note Odoo version (14.0, 15.0, 16.0, 17.0, 18.0, 19.0)
- Community vs Enterprise edition
- Installed modules and their versions
- Custom modules or modifications

### Environment Details
- Deployment type (Odoo.sh, on-premise, hosting provider)
- Database size and complexity
- Number of users
- Integration points (payment gateways, shipping, accounting)

## Duplicate Detection

Before creating new escalation, check for:
- Similar error messages in recent tickets
- Same module/feature reported by other customers
- Known bugs in internal tracker (Jira)
- Existing KB articles

## Information Gathering Checklist

### For Technical Issues
- [ ] Odoo version and edition
- [ ] Affected module(s)
- [ ] Error message (full text)
- [ ] Reproduction steps
- [ ] Screenshots or screen recording
- [ ] Browser and OS (if frontend issue)
- [ ] When did it start happening?
- [ ] Any recent changes (upgrades, new modules, configuration)?

### For Configuration Questions
- [ ] What are you trying to achieve?
- [ ] What have you tried already?
- [ ] Current configuration screenshots
- [ ] Specific use case or workflow

### For Feature Requests
- [ ] Business problem trying to solve
- [ ] Current workaround (if any)
- [ ] Impact if not implemented
- [ ] Number of users affected

## Best Practices

1. **Always acknowledge** - Confirm receipt within SLA
2. **Set expectations** - Tell customer what happens next
3. **Ask clarifying questions** - Get complete information upfront
4. **Search first** - Check for existing solutions
5. **Document everything** - Update ticket with findings
6. **Follow up** - Don't let tickets go stale
7. **Close the loop** - Confirm resolution with customer
8. **Update KB** - Turn common issues into articles
