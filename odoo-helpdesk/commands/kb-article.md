# Command: /kb-article

Create a knowledge base article from a resolved Odoo ticket or common support question.

## Usage

```
/kb-article <topic or ticket reference>
```

## What It Does

1. **Analyzes** the resolved ticket or topic
2. **Structures** the article for searchability
3. **Writes** clear, step-by-step instructions
4. **Includes** troubleshooting tips
5. **Optimizes** for Odoo Knowledge or Notion

## Examples

```
/kb-article How to configure webhook notifications - resolved ticket #1234
```

```
/kb-article Email template configuration - got this question 5 times this month
```

```
/kb-article Troubleshooting blank invoice reports
```

## Output Format

```markdown
# [Article Title]

**Category:** [Billing/Technical/Configuration/etc.]
**Applies to:** [Odoo version(s), module(s)]
**Last updated:** [Date]

## Overview
[Brief description of what this article covers and when to use it]

## Prerequisites
- [Requirement 1]
- [Requirement 2]

## Step-by-Step Instructions

### Step 1: [Action]
[Detailed instructions]

Screenshot placeholder: `[Screenshot: description]`

### Step 2: [Action]
[Detailed instructions]

### Step 3: [Action]
[Detailed instructions]

## Verification
How to verify it worked:
1. [Check 1]
2. [Check 2]

## Common Issues

### Issue: [Problem]
**Cause:** [Why it happens]
**Solution:** [How to fix]

### Issue: [Problem]
**Cause:** [Why it happens]
**Solution:** [How to fix]

## Related Articles
- [Link to related article 1]
- [Link to related article 2]

## Technical Details
[Optional: For technical users - API calls, database queries, etc.]

## See Also
- [Related documentation]
- [Video tutorial if available]

---

**Tags:** [tag1], [tag2], [tag3]
**Search terms:** [keyword1], [keyword2], [keyword3]
**Source ticket:** #[ticket_id]
```

## Skills Used

- `knowledge-management/SKILL.md` - Article structure and writing guidelines
- `response-drafting/SKILL.md` - Clear communication

## Article Types

### How-To
Configuration, setup, or workflow instructions.

### Troubleshooting
Diagnosing and resolving common errors.

### Reference
Feature documentation, API specs, field descriptions.

### FAQ
Common questions with concise answers.

## Tips for Good KB Articles

- **Searchable titles** - Use terms customers would search for
- **Clear prerequisites** - Don't assume knowledge
- **Screenshots** - Mark where screenshots are needed
- **Test the steps** - Verify they work before publishing
- **Keep updated** - Note which Odoo versions this applies to
- **Link related articles** - Create a web of knowledge
- **Add tags** - Help with discovery

## Where to Publish

- **Odoo Knowledge** - Internal wiki module
- **Notion** - Team documentation
- **Odoo Website** - Public help center (if configured)
- **Guru** - If using Guru integration
