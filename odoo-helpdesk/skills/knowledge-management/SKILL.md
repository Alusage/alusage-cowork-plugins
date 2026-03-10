# Skill: Knowledge Management for Odoo Helpdesk

This skill provides guidelines for creating, maintaining, and organizing knowledge base articles from support interactions.

## When to Create a KB Article

### High Priority
- **Frequent questions** - Asked 3+ times per month
- **Complex solutions** - Takes >10 minutes to explain
- **Critical procedures** - Important workflows everyone should know
- **Workarounds** - Temporary solutions for known issues
- **Onboarding topics** - Common new customer questions

### Medium Priority
- **Module configuration** - How to set up specific features
- **Integration guides** - Connecting to third-party services
- **Troubleshooting guides** - Common error resolution
- **Best practices** - Recommended ways to use features

### Lower Priority
- **Edge cases** - Rare but documented solutions
- **Version-specific** - Important for specific Odoo versions
- **Advanced topics** - Deep dives for power users

### Don't Create
- **One-off questions** - Unlikely to be asked again
- **Customer-specific** - Only applies to one setup
- **Temporary** - Will be outdated soon
- **Already documented** - Better to link to existing docs

## Article Structure

### Standard Template

```markdown
# [Action-Oriented Title]

**Category:** [Technical/Configuration/Billing/etc.]
**Module(s):** [Affected Odoo modules]
**Applies to:** Odoo [version(s)]
**Difficulty:** [Beginner/Intermediate/Advanced]
**Estimated time:** [X minutes]
**Last updated:** [YYYY-MM-DD]

## Overview
[2-3 sentences: What this article covers and when to use it]

## Prerequisites
- [ ] [Requirement 1, e.g., "Administrator access required"]
- [ ] [Requirement 2, e.g., "Sales module installed"]
- [ ] [Requirement 3, e.g., "Company settings configured"]

## Instructions

### Step 1: [Action Verb - What to Do]
[Detailed explanation]

**Path:** Settings → [Menu path]

[Screenshot placeholder: `![Description](screenshot-1.png)`]

**Tip:** [Helpful hint or common mistake to avoid]

### Step 2: [Next Action]
[Detailed explanation]

**Configuration:**
- **Field Name:** [What to enter/select]
- **Field Name:** [What to enter/select]

### Step 3: [Final Action]
[Detailed explanation]

## Verification
How to confirm it worked:

1. [Check 1 - where to look and what to see]
2. [Check 2 - test action]
3. [Expected result]

## Common Issues

### Issue: [Problem Description]
**Symptom:** [How it appears]
**Cause:** [Why it happens]
**Solution:** [How to fix it]

### Issue: [Another Problem]
**Symptom:** [How it appears]
**Cause:** [Why it happens]
**Solution:** [How to fix it]

## Advanced Configuration
[Optional section for power users]

[Advanced options, customization, API usage, etc.]

## Related Articles
- [Link to related article 1]
- [Link to related article 2]

## See Also
- [Odoo official documentation link]
- [Video tutorial if available]
- [Community forum discussion if relevant]

## Feedback
Last reviewed: [Date]
Reviewed by: [Name]

**Was this helpful?** If you have feedback or this didn't work for you, please reply to support ticket or contact [email/channel].

---
**Tags:** `[tag1]`, `[tag2]`, `[tag3]`, `[module-name]`, `[version]`
**Internal:** Source ticket #[ticket_id] | Created by [Author] | [YYYY-MM-DD]
```

## Article Types

### How-To Guide
**Purpose:** Step-by-step instructions for accomplishing a task
**Format:** Numbered steps with screenshots
**Example:** "How to Configure Email Templates in Odoo"

**Structure:**
1. Clear objective
2. Prerequisites
3. Step-by-step instructions
4. Verification steps
5. Troubleshooting

### Troubleshooting Guide
**Purpose:** Diagnose and fix specific errors
**Format:** Problem → Cause → Solution

**Structure:**
1. Error description
2. Possible causes (most common first)
3. Diagnostic steps
4. Solutions for each cause
5. Prevention tips

### Reference Article
**Purpose:** Document features, fields, or concepts
**Format:** Descriptive with examples

**Structure:**
1. What it is
2. When to use it
3. How it works
4. Examples
5. Related concepts

### FAQ
**Purpose:** Quick answers to common questions
**Format:** Q&A pairs

**Structure:**
1. Question (natural language)
2. Brief answer
3. Link to detailed article if needed

## Writing Guidelines

### Titles
✅ **Good:**
- "How to Configure Automated Email Notifications"
- "Troubleshooting Blank Invoice PDF Reports"
- "Understanding Odoo's Multi-Company Rules"

❌ **Bad:**
- "Email Configuration" (too vague)
- "Problem with Invoices" (not specific)
- "Info about Companies" (not action-oriented)

### Style
- **Active voice:** "Click the button" not "The button should be clicked"
- **Present tense:** "Odoo sends emails" not "Odoo will send emails"
- **Second person:** "You can configure" not "Users can configure"
- **Short sentences:** Break complex ideas into simple steps
- **Bullet points:** For lists and options
- **Numbered lists:** For sequential steps

### Language Level
- **Beginner articles:** No jargon, explain everything
- **Intermediate:** Can assume basic Odoo knowledge
- **Advanced:** Can use technical terminology

### Screenshots
- **When to include:**
  - Complex interfaces
  - Specific field locations
  - Verification steps
  - Before/after comparisons

- **Best practices:**
  - Use sample data (not customer data)
  - Highlight relevant area (arrows, boxes)
  - Use consistent Odoo version for all screenshots
  - Crop to relevant area
  - Include enough context to orient user

- **Placeholder format:**
```
![Screenshot description](screenshot-filename.png)
```

### Code and Commands
Use code blocks for:
- File paths: `/opt/odoo/addons/custom_module`
- Commands: `sudo systemctl restart odoo`
- API examples: `model.search([('field', '=', 'value')])`
- Configuration: XML, Python code snippets

Example:
```python
# Example: Search for active partners
partners = self.env['res.partner'].search([
    ('active', '=', True),
    ('customer_rank', '>', 0)
])
```

## Searchability Best Practices

### SEO for KB
- **Title:** Include key search terms
- **First paragraph:** Summarize with natural language
- **Headers:** Use descriptive H2/H3 with keywords
- **Tags:** Add relevant tags (module names, features, error codes)
- **Meta description:** Add short summary at top

### Common Search Terms
Include these where relevant:
- Error messages (exact text)
- Module names
- Feature names (both technical and user-facing)
- Common misspellings
- Alternative terms

**Example:**
Article about email templates should include:
- "email template"
- "email configuration"
- "automated email"
- "email notification"
- "mail template" (technical term)

## Organization

### Category Structure
```
Knowledge Base/
├── Getting Started
│   ├── Installation
│   ├── Initial Setup
│   └── User Management
├── Sales
│   ├── CRM
│   ├── Quotations
│   └── Sales Orders
├── Accounting
│   ├── Invoicing
│   ├── Payments
│   └── Reports
├── Inventory
├── Manufacturing
├── Website/eCommerce
├── Helpdesk
├── Technical
│   ├── API
│   ├── Customization
│   └── Performance
└── Troubleshooting
    ├── Common Errors
    ├── Module-Specific
    └── Performance Issues
```

### Tagging System
Use consistent tags:
- **Module:** `#sales`, `#accounting`, `#inventory`
- **Version:** `#v16`, `#v17`, `#v18`
- **Difficulty:** `#beginner`, `#advanced`
- **Type:** `#how-to`, `#troubleshooting`, `#reference`
- **Feature:** `#email`, `#reporting`, `#workflow`

## Maintenance

### Review Schedule
- **High-traffic articles:** Quarterly
- **Version-specific:** At each major release
- **Troubleshooting:** When issue is fixed in new version
- **All articles:** Annually

### Update Checklist
- [ ] Still accurate for current Odoo version?
- [ ] Screenshots still match current UI?
- [ ] Links still work?
- [ ] Better solution available now?
- [ ] Missing common questions from recent tickets?
- [ ] Tags still appropriate?

### Deprecation
When to archive an article:
- Feature removed from Odoo
- Replaced by better article
- No longer relevant (version too old)
- Better documented in official docs

**Don't delete** - Redirect or leave with "deprecated" notice and link to new article.

## Measuring Success

### Article Metrics
- **Page views** - Is it being found?
- **Time on page** - Are people reading it?
- **Thumbs up/down** - Is it helpful?
- **Ticket deflection** - Did tickets decrease?
- **Search ranking** - Does it show up for key terms?

### Red Flags
- High views but low satisfaction → Article needs improvement
- Low views but frequent topic → Searchability issue
- High bounce rate → Not what users expected from title
- Common question still generating tickets → Article not found or incomplete

## Creating from Tickets

### During Resolution
1. Note if this could be a KB article
2. Document solution clearly in ticket
3. Save screenshots while troubleshooting
4. Note customer's original question (for title)

### After Resolution
1. Check if this is frequent enough
2. Generalize the solution (remove customer-specific details)
3. Write in template format
4. Have colleague review
5. Publish and link in original ticket

### Quick Capture Template
```
KB Article Idea:
- Title: [Customer's question as they asked it]
- Problem: [What they were trying to do]
- Solution: [How we solved it]
- Frequency: [How often do we see this?]
- Source ticket: #[ticket_id]
- Complexity: [How long to explain?]
```

## Quality Standards

Before publishing:
- [ ] Title is clear and searchable
- [ ] Prerequisites are listed
- [ ] Steps are numbered and detailed
- [ ] Screenshots show clearly what to do
- [ ] Verification steps included
- [ ] Common issues addressed
- [ ] Related articles linked
- [ ] Tags added
- [ ] Version specified
- [ ] No customer-specific information
- [ ] Proofread for typos and clarity
- [ ] Tested by following own instructions

## Tools and Platforms

### Where to Publish
- **Odoo Knowledge** (built-in wiki)
- **Notion** (team documentation)
- **Guru** (in-app knowledge base)
- **Confluence** (enterprise wiki)
- **Odoo Website** (public help center)

### Formatting Tools
- Markdown editors for drafting
- Screenshot tools (with annotation)
- Screen recording for video guides
- Diagram tools (draw.io, Lucidchart)

## Pro Tips

1. **Write as you troubleshoot** - Capture steps in real-time
2. **Use customer's language** - How they describe the problem
3. **Include the "why"** - Help users understand, not just follow steps
4. **Link liberally** - To related articles and official docs
5. **Update in batches** - Review multiple articles at once for consistency
6. **Crowdsource feedback** - Ask team which articles are outdated
7. **Celebrate good KB** - Recognize team members who create great articles
8. **Make it easy** - Have templates and tools ready

The best knowledge base is the one people actually use. Keep it current, searchable, and genuinely helpful.
