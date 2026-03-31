# Polish Presets Reference

Predefined scenarios and goals for the `/polish` skill. Users can select from these during setup or define custom ones.

---

## Scenarios

Scenarios define the **context** of the communication. They influence word choice, formality level, and cultural norms.

### Professional

| Scenario | Description | Tone Defaults |
|----------|-------------|---------------|
| Workplace general | Slack, Teams, emails to coworkers | Clear, friendly-professional |
| Manager/leadership | Messages to managers, skip-levels, execs | Polished, confident, concise |
| Client/external | Emails to clients, vendors, partners | Formal, courteous, precise |
| Code review | PR comments, technical feedback | Direct, constructive, specific |
| Job application | Cover letters, recruiter messages, follow-ups | Professional, enthusiastic, polished |

### Academic

| Scenario | Description | Tone Defaults |
|----------|-------------|---------------|
| Academic writing | Papers, essays, research proposals | Formal, precise, objective |
| Professor/advisor | Emails to professors, advisors | Respectful, clear, concise |
| Peer collaboration | Messages to classmates, study groups | Friendly, clear |
| Conference | Abstracts, talk proposals, speaker bios | Formal, authoritative |

### Day-to-Day

| Scenario | Description | Tone Defaults |
|----------|-------------|---------------|
| Casual chat | Texts, social messages, group chats | Natural, relaxed, warm |
| Social media | X/Twitter, LinkedIn, Reddit posts | Engaging, authentic, platform-appropriate |
| Dating | App messages, personal introductions | Warm, genuine, confident |

### Specific Situations

| Scenario | Description | Tone Defaults |
|----------|-------------|---------------|
| Doctor/medical | Describing symptoms, asking medical questions | Clear, precise, factual |
| Legal/government | Visa applications, official forms, government emails | Formal, exact, thorough |
| Customer service | Complaints, requests, follow-ups | Firm but polite, clear |
| Real estate | Agent communication, lease questions | Professional, specific |
| Travel | Hotel/airline requests, booking issues | Polite, clear, concise |
| Negotiation | Salary, contracts, disputes | Confident, diplomatic, firm |
| Parenting/school | Teacher emails, school admin communication | Warm, respectful, clear |

---

## Polish Goals

Goals define **what the polishing should optimize for**. Multiple goals can be combined.

### Preset Goals

| Goal | What It Does | Example |
|------|-------------|---------|
| Native-sounding | Remove L2 patterns, make phrasing natural and idiomatic | "I want to ask about..." → "I was wondering about..." |
| Formal | Elevate register, use professional vocabulary | "Can you help me with this?" → "Could you assist me with this matter?" |
| Casual | Lower register, make it conversational and warm | "I would like to inform you..." → "Just wanted to let you know..." |
| Concise | Cut filler, tighten wording, reduce word count | "I wanted to reach out to you to see if..." → "Would you be open to..." |
| Assertive | Strengthen hedging language, sound confident | "I think maybe we could..." → "We should..." |
| Diplomatic | Soften direct statements, add cushioning | "This is wrong." → "I see this differently — could we revisit this?" |
| Engaging | Make it vivid, add hooks, improve rhythm | Flat prose → Varied sentence length, stronger verbs, better flow |
| Grammar only | Fix errors without changing style or voice | Minimal intervention — spelling, punctuation, grammar only |

### Goal Combinations

Common combos that work well together:

- **Professional email**: Native-sounding + Formal + Concise
- **Slack to manager**: Native-sounding + Concise + Assertive
- **Casual message**: Native-sounding + Casual
- **LinkedIn post**: Native-sounding + Engaging + Concise
- **Complaint email**: Formal + Assertive + Diplomatic
- **Academic paper**: Formal + Concise + Grammar only

---

## Output Formats

| Format | Description | Best For |
|--------|-------------|----------|
| Polished only | Just the clean result, ready to copy | Speed — when you trust the skill |
| Side-by-side diff | Original vs polished with changes marked | Learning — understanding what changed |
| Polished + explanation | Result first, then brief explanation of changes | Balance — quick result with optional learning |

---

## Auto-Detection Signals

The skill auto-detects text type from structural cues. No user input needed.

| Signal | Detected Type |
|--------|---------------|
| `Subject:` or `Hi/Dear [name],` + signature | Email |
| Short, no greeting, informal punctuation | Chat/Slack message |
| `@mentions`, hashtags, character count ~280 | Tweet/X post |
| Bullet points, headers, technical terms | Doc/PR description |
| Long paragraphs, citations, formal structure | Academic writing |
| `Dear Sir/Madam`, reference numbers | Formal/official letter |

When detection is ambiguous, polish as general text without imposing a format.
