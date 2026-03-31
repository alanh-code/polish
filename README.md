# Polish

An agent CLI skill that polishes your text — messages, emails, posts, docs — with one command.

Built for non-native speakers who want to sound natural without repeated copy-pasting for polishment every time.

## Before / After

**Slack message:**
```
/polish sorry for bother you, can I ask did you have a chance to review my PR?
```
> Sorry to bother you — have you had a chance to review my PR?

**Message:**
```
/polish I just realized I didn't get receipt yesterday when I go to pick up the order. Is it possible to get one? I want to keep all record together.
```
> I just realized I didn't get a receipt yesterday when I picked up the order. Would it be possible to get one? I'd like to keep all my records together.

**Email:**
```
/polish Dear Sir, I am writing this email for request a refund. The product is having damage on it.
```
> Dear Sir, I'd like to request a refund. The product I received is damaged.

**LinkedIn post:**
```
/polish When I first came I was very confuse and didn't knew anybody. I am very appreciate for this opportunity.
```
> When I first started, I was completely lost and didn't know anyone. I'm really grateful for this opportunity.

**Chinese:**
```
/polish 你好,我想问一下这个快递大概什么时候能到
```
> 你好，请问这个快递大概什么时候能到呢？

## Features

- **Auto-detects language** — Polish in whatever language you write. English, Chinese, Japanese, anything.
- **Auto-detects text type** — Email, Slack message, tweet, doc. No need to tell it.
- **Preserves structure** — Greeting stays a greeting. Bullets stay bullets. Signature stays a signature.
- **Preserves voice** — You sound like a better you, not like an AI.
- **Multiple profiles** — Switch between `work`, `casual`, `academic`, or any custom profile.
- **Configurable output** — Polished text only, or with change explanations.

## Installation

Clone directly from GitHub into your agent skills directory.

For Claude Code:

```bash
git clone https://github.com/alanh-code/polish.git ~/.claude/skills/polish
```

For Codex:

```bash
git clone https://github.com/alanh-code/polish.git ~/.codex/skills/polish
```

Or copy the files manually into your agent's skill directory:

```bash
mkdir -p ~/.claude/skills/polish
cp SKILL.md presets.md ~/.claude/skills/polish/
```

Then type `/polish` in your agent CLI.

## Usage

### First run

`/polish` triggers a one-time setup — pick your scenario, goals, and output format. Takes 30 seconds.

### After setup

```
/polish <your text here>
```

That's it. One command, polished output.

### Commands

| Command | What it does |
|---------|-------------|
| `/polish <text>` | Polish with last-used profile |
| `/polish --profile work <text>` | Polish with a specific profile |
| `/polish --setup` | Create a new profile |
| `/polish --profiles` | List all saved profiles |
| `/polish --edit <name>` | Edit a profile |
| `/polish --delete <name>` | Delete a profile |

### Profiles

Profiles save your preferences so you don't configure every time:

```json
{
  "work": {
    "scenario": "professional",
    "goals": ["native-sounding", "concise", "formal"]
  },
  "casual": {
    "scenario": "day-to-day",
    "goals": ["native-sounding", "casual"]
  }
}
```

No flag = last-used profile. `--profile <name>` switches.

## Architecture

| File | Purpose | Loaded when |
|------|---------|-------------|
| `SKILL.md` | Core workflow — setup, detection, polishing rules | Always |
| `presets.md` | Scenario/goal presets, auto-detection signals | Setup + polishing |
| `user-config.json` | Your saved profiles (generated at runtime, not in repo) | Every run |

## Requirements

- An agent CLI that supports skills, such as Claude Code or Codex.

## License

MIT
