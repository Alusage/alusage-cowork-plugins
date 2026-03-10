# Skill: Response Drafting for Odoo Helpdesk

This skill provides templates, tone guidelines, and best practices for drafting customer-facing responses in Odoo Helpdesk.

## Communication Principles

### Always
- **Empathize** - Acknowledge the customer's frustration or concern
- **Own it** - Take responsibility, even if it's not your fault
- **Be specific** - Give concrete next steps and timelines
- **Follow up** - Set clear expectations for next contact
- **Keep it human** - Write like a person, not a bot

### Never
- **Blame** - Don't blame the customer, other teams, or the product
- **Overpromise** - Don't commit to timelines you can't meet
- **Use jargon** - Explain technical terms in plain language
- **Leave them hanging** - Always indicate what happens next
- **Auto-close** - Confirm resolution before closing ticket

## Tone Guidelines by Situation

### P1 - Critical Issue
**Tone:** Empathetic, urgent, ownership

**Structure:**
1. Immediate acknowledgment of severity
2. Apology for impact
3. Confirmation we're treating as top priority
4. Specific person working on it
5. Concrete timeline for next update
6. Direct contact info (phone/Slack if appropriate)

**Example:**
```
I understand your [module] is completely down and this is blocking your team. I'm very sorry for the impact this is having on your business.

I've immediately escalated this to our senior engineer [Name], who is investigating right now. This is our top priority.

I'll update you by [specific time, within 2 hours] with our findings and action plan. If you need to reach me directly, I'm available at [phone/email].
```

### P2 - High Priority
**Tone:** Professional, proactive, clear timeline

**Structure:**
1. Acknowledge the issue
2. Brief apology
3. Explain what we're doing
4. Timeline for resolution or update
5. Workaround if available

**Example:**
```
Thank you for reporting this issue with [feature]. I can see how this is disrupting your workflow and I apologize for the inconvenience.

I've created a bug report and our engineering team is reviewing it today. I'll have an update for you by [tomorrow/end of week] on timeline for the fix.

In the meantime, here's a workaround: [workaround steps]
```

### P3/P4 - Standard Issues
**Tone:** Helpful, educational

**Structure:**
1. Confirm understanding
2. Provide solution or explanation
3. Verification steps
4. Offer additional help

**Example:**
```
Thanks for your question about [topic]! I'd be happy to help with that.

Here's how to [solve the problem]:
[Step-by-step instructions]

Let me know if that works for you or if you have any questions!
```

### Feature Requests
**Tone:** Appreciative, transparent

**Structure:**
1. Thank them for the suggestion
2. Explain current status/roadmap (if applicable)
3. Explain process for feature requests
4. Offer workaround if available
5. Invite continued feedback

**Example:**
```
Thank you for this suggestion! I can see how [feature] would be valuable for your workflow.

I've added this to our feature request tracker. Our product team reviews these quarterly when planning the roadmap. While I can't commit to a timeline, your use case helps us prioritize.

In the meantime, you might be able to achieve something similar by [workaround].

I'd love to hear more about your specific use case if you have time for a quick call.
```

### Escalated/Frustrated Customers
**Tone:** Empathetic, de-escalating, senior ownership

**Structure:**
1. Deep acknowledgment of frustration
2. Sincere apology
3. Elevate to senior person (if appropriate)
4. Specific, aggressive timeline
5. Offer call/meeting
6. Commitment to prevent recurrence

**Example:**
```
[Customer name], I want to sincerely apologize for the experience you've had with this issue. It's completely unacceptable that [problem] has persisted for [duration], and I understand your frustration.

I'm [name], [senior title], and I'm personally taking ownership of getting this resolved for you. Here's what we're doing:

1. [Specific action] - [person responsible] - [completion time]
2. [Specific action] - [person responsible] - [completion time]

I'd like to schedule a call with you [today/tomorrow] to discuss this in detail and ensure we have a clear path forward. Are you available at [specific times]?

I'll also be reviewing our internal processes to understand how this happened and prevent it from occurring again.
```

## Response Templates by Type

### Acknowledged - Investigating
```
Thanks for reporting this, [name]. I'm looking into it now and will update you by [time] with my findings.

Quick questions to help me troubleshoot:
1. [Question]
2. [Question]
```

### Solution Provided
```
I found the issue! [Explanation of what was wrong]

Here's how to resolve it:
[Steps]

Can you try this and let me know if it works?
```

### Escalated to Engineering
```
I've investigated this and confirmed it's a bug in [module]. The good news is we can reproduce it, which means our engineering team can fix it.

I've created bug ticket #[number] and it's prioritized for our next release ([timeframe]). I'll keep you updated on progress.

[Optional workaround if available]
```

### Needs More Information
```
Thanks for your patience. To help resolve this, I need a bit more information:

1. [Specific question]
2. [Specific question]
3. [Optional: screenshot request]

This will help me [explain why you need this info].
```

### Closing Ticket
```
Great! I'm glad that resolved it.

I'm marking this ticket as solved. If you run into this again or have any other questions, just reply to this ticket or open a new one.

[Optional: mention KB article if created]
```

## Odoo-Specific Language

### Technical Terms to Explain
- **Form view / List view** → "the detailed screen" / "the list page"
- **Server action** → "automated action"
- **Scheduled action** → "scheduled task"
- **Record rule** → "security rule"
- **Chatter** → "message history" or "discussion tab"
- **Smart button** → "the counter button at the top"
- **Many2one / One2many** → just explain the relationship in plain language

### Common Scenarios

**Module Not Installed**
```
It looks like the [Module Name] module isn't activated in your instance. This feature requires that module.

You can activate it by going to Apps → searching for "[Module Name]" → clicking Install.

Note: If you're on Odoo.sh or hosted, you may need [permission level] access to install modules. Let me know if you need help with that!
```

**Access Rights Issue**
```
This appears to be a permissions issue. The [action] requires [permission level] access to the [model] model.

Your admin can grant this by:
1. Settings → Users & Companies → Users
2. Find your user
3. Access Rights tab
4. [Specific permission to grant]

Would you like me to create a summary you can forward to your admin?
```

**Configuration Missing**
```
It looks like [feature] hasn't been configured yet. Here's how to set it up:

[Configuration steps]

Once that's configured, you'll be able to [desired action].
```

## Writing for Different Audiences

### End Users
- Use plain language
- Include screenshots
- Step-by-step instructions
- Anticipate follow-up questions

### Administrators
- Can reference technical concepts
- Focus on configuration and settings
- Mention implications for other users
- Link to official documentation

### Developers
- Can use technical terminology
- Reference code, models, fields
- Provide API examples if relevant
- Link to developer documentation

## Quality Checklist

Before sending:
- [ ] Did I acknowledge their issue?
- [ ] Did I explain what I'm doing or what they should do?
- [ ] Did I give a timeline for next update?
- [ ] Did I offer additional help?
- [ ] Is the tone appropriate for the situation?
- [ ] Did I avoid jargon or explain technical terms?
- [ ] Did I proofread for typos and clarity?
- [ ] Would I be satisfied with this response as a customer?

## Follow-Up Timing

### When to Follow Up
- **P1:** Every 2 hours until resolved
- **P2:** Daily
- **P3:** Every 2-3 days
- **P4:** Weekly

### If Customer Goes Silent
After 3-5 days of no response:
```
Hi [name],

I wanted to check in on this. Were you able to try the solution I suggested? If you're still having issues, I'm happy to help further.

If I don't hear back in the next few days, I'll mark this as resolved, but you can always reopen it by replying.
```

## Internal Notes

Always add internal notes (visible only to team) with:
- What you actually did
- Sources you checked
- Time spent
- Related tickets
- Follow-up needed

This helps the next person who picks up the ticket.
