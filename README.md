# Skill Creator

When you ask Claude for something it can't do—video generation, reminders, price alerts—it builds the capability instead of refusing.

```
You: "Remind me to water my plants every 3 days"

Claude: [creates reminders skill, sets up your reminder]

        "Done. You now have a reminders skill—works for
        anything: birthdays, meds, deadlines. See /skills."
```

You get what you asked for, plus a reusable skill.

## Install

```bash
mkdir -p ~/.claude/skills
cp -r .claude/skills/skill-creator ~/.claude/skills/
```

## Use

Ask for something Claude can't do natively:

- "Remind me to take my medication"
- "Alert me when Bitcoin drops below 90k"
- "Make me an AI video"
- "Turn off my lights"

Claude builds the skill, completes your request, and you can reuse it anytime.

## How it works

1. You ask for something requiring external tools/APIs
2. Claude creates a general-purpose skill (not task-specific)
3. Guides you through any setup (API keys, etc.)
4. Completes your original request
5. Skill persists for future use

## License

Apache 2.0
