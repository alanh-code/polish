---
name: polish
description: Polish any text for clarity, tone, and natural flow. Supports multiple profiles (work, casual, academic, custom), auto-detects text type and language, preserves structure. Use when the user wants to refine a message, email, post, or any written text.
---

# Polish

Polish any text — messages, emails, posts, docs — with consistent quality and minimal friction.

## Core Principles

1. **Preserve Voice** — Polish the text, don't rewrite it. The result should sound like a better version of the user, not like an AI.
2. **Auto-Detect Everything** — Text type (email, chat, post) and language are detected automatically. Never ask the user to classify their own text.
3. **One Command, Done** — After initial setup, `/polish <text>` should return the result immediately. No follow-up questions during normal use.
4. **Structure Preservation** — If the input has structure (subject line, greeting, body, signature), maintain it. Polish each section with appropriate tone.
5. **No Internal State Exposed** — Never show phase names, config detection status, or internal logic to the user. No messages like "Phase 0: checking config..." or "No user-config.json found." The user should only see setup questions, polished output, or profile confirmations — nothing else.

---

## Phase 0: Detect Mode

Check if `user-config.json` exists in the skill directory.

- **Config exists** → Read it, load the last-used profile, go to **Phase 2**.
- **Config does not exist** → First-time user. Go to **Phase 1**.
- **User passed `--setup`** → Go to **Phase 1** (create new profile).
- **User passed `--profiles`** → List all saved profiles and their settings. Done.
- **User passed `--profile <name>`** → Switch to that profile, update `lastUsedProfile` in config, go to **Phase 2**.

---

## Phase 1: First-Time Setup

Create a profile through a guided setup. Use AskUserQuestion to collect preferences in batches.

### Step 1.1: Scenario

**Ask in a single AskUserQuestion call:**

**Question 1 — Scenario** (header: "Scenario"):
What's the primary context for this profile? Options:
- "Professional — workplace, clients, managers"
- "Academic — papers, professors, conferences"
- "Day-to-day — casual, social, personal"

User can type a custom scenario (e.g., "doctor visits", "customer support", "dating apps").

### Step 1.2: Polish Goals & Output

**Ask in a single AskUserQuestion call with multiSelect where noted:**

**Question 1 — Goals** (header: "Goals", multiSelect: true):
What should polishing optimize for? Options:
- "Native-sounding" — Remove L2 patterns, make it flow naturally
- "Concise" — Cut filler, tighten wording
- "Formal" — Professional, elevated register
- "Casual" — Relaxed, conversational, warm

User can type custom goals (e.g., "sound more confident", "softer tone for sensitive topics").

**Question 2 — Output format** (header: "Output"):
How should the polished text be shown? **Use the `preview` field on each option** to show a visual example:

- "Polished only (Recommended)" — Just the clean result, ready to copy
  - preview: `Could you explain this as if I have no technical background at all?`
- "Polished + explanation" — Result first, then brief note on what changed
  - preview: `Could you explain this as if I have no technical background at all?\n\nChanges: tightened phrasing, replaced "supposed" with natural idiom.`
- "Side-by-side diff" — Original vs polished with changes highlighted
  - preview: `Original: can you explain this to me supposed i have no techinical background at all\nPolished: Could you explain this as if I have no technical background at all?\n\nKey changes:\n- "supposed" → "as if": correct idiom\n- "techinical" → "technical": typo fix`

### Step 1.3: Name & Save Profile

**Ask in a single AskUserQuestion call:**

**Question 1 — Profile name** (header: "Profile"):
Name this profile. Options:
- "work" — Professional communication
- "casual" — Day-to-day, friends, social
- "academic" — Papers, professors, research

User can type a custom name. Profile names should always be lowercase.

Write the selected profile to `user-config.json`. The exact values should come from the user's setup answers. Use this structure:

```json
{
  "lastUsedProfile": "[profile-name]",
  "profiles": {
    "[profile-name]": {
      "scenario": "[selected-or-custom-scenario]",
      "goals": ["[selected-goal]"],
      "outputFormat": "[selected-output-format]",
      "customInstructions": "[optional-custom-instructions]"
    }
  }
}
```

If custom goals or scenario were provided as free text, store them in `customInstructions`.

Confirm to user: **"Profile '[name]' saved. You can now use `/polish <text>`."**

If the user provided text along with the `/polish` command, proceed to **Phase 2**. If no text was provided, stop here — do not suggest examples or show any additional messages.

---

## Phase 2: Polish

This is the core execution phase. It should be fast — no questions asked.

### Step 2.1: Parse Input

Extract the text to polish from the user's input. The text is everything after `/polish` (and any flags).

Examples:
- `/polish can you explain this to me like I have no technical background`
- `/polish --profile work Dear team, I want to sharing the update about project.`

If no text is provided (e.g., user selected `/polish` from auto-complete and hit enter), prompt the user using AskUserQuestion with a single free-text question (header: "Text", question: "What text would you like to polish?"). Do not output any other message — just ask for the text.

### Step 2.2: Auto-Detect

Detect from the input text (do NOT ask the user):

1. **Language** — What language is the text written in? Polish in the same language.
2. **Text type** — Email, chat message, social post, document, etc. Use signals from [presets.md](presets.md) auto-detection table.
3. **Structure** — Does it have identifiable sections (subject, greeting, body, signature, bullet points)?

### Step 2.3: Apply Polish

Using the active profile's settings, polish the text following these rules:

**Voice preservation rules:**
- Keep the user's sentence structure when it's already natural
- Preserve their word choices when they're correct and intentional
- Maintain their punctuation style (e.g., if they use em dashes, keep using em dashes)
- Don't add formality the profile doesn't call for
- Don't inject idioms or expressions the user wouldn't naturally use
- If the input is already well-written, make minimal changes. Don't polish for the sake of polishing.

**Structure preservation rules:**
- If input has a greeting, keep a greeting
- If input has a sign-off, keep a sign-off
- If input uses bullet points, keep bullet points
- If input has a subject line, keep a subject line
- Match the input's paragraph structure unless consolidation clearly improves readability

**Language rules:**
- Polish in the same language as the input
- **All output must be in the same language as the input** — including labels like "Changes:", "Original:", "Polished:", "Key changes:". For example, Chinese input → "修改：", not "Changes:"
- If the input mixes languages (code-switching), preserve the mix unless it hurts clarity
- Respect language-specific conventions (e.g., formal/informal pronouns in French, German, Japanese, Korean)

### Step 2.4: Output

Based on the profile's `outputFormat`:

**Polished only:**
Output the polished text directly. Nothing else. The user should be able to copy it immediately.

**Polished + explanation:**
Output the polished text first, then the change log wrapped in `< >` to distinguish it from the polished text. Format:

```
[polished text]

<Changes: [brief description of key changes]>
```

**Side-by-side diff:**
Output original and polished, with the entire diff section wrapped in `< >`:

```
<Original: [original text]
Polished: [polished text]

Key changes:
- [change 1]: [reason]
- [change 2]: [reason]>
```

For long text (emails, docs), show the full polished version first, then a summary of changes below — not an inline diff of every sentence.

---

## Command Reference

| Command | Action |
|---------|--------|
| `/polish <text>` | Polish text using last-used profile |
| `/polish --profile <name> <text>` | Polish using a specific profile (and switch to it) |
| `/polish --setup` | Create a new profile or re-run setup |
| `/polish --profiles` | List all saved profiles with their settings |
| `/polish --edit <name>` | Edit an existing profile's settings |
| `/polish --delete <name>` | Delete a profile |

---

## Config File Format

Stored at `polish/user-config.json` in the skill directory.

This is the config shape. Values are generated from the user's setup answers.

```json
{
  "lastUsedProfile": "[profile-name]",
  "profiles": {
    "[profile-name]": {
      "scenario": "[selected-or-custom-scenario]",
      "goals": ["[selected-goal]"],
      "outputFormat": "[selected-output-format]",
      "customInstructions": "[optional-custom-instructions]"
    }
  }
}
```

---

## Supporting Files

| File | Purpose | When to Read |
|------|---------|-------------|
| [presets.md](presets.md) | Scenario and goal presets, auto-detection signals | Phase 1 (setup) and Phase 2 (auto-detect) |
| user-config.json | User profiles and preferences | Phase 0 (detect mode) |
