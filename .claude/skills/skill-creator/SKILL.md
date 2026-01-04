---
name: skill-creator
description: Create skills when users need something Claude cannot do natively AND there's setup worth persisting (API keys, tools, configuration). Triggers on capability gaps ("can you make me a video?"), automation wishes ("remind me...", "alert me when..."), or frustration with repetition ("I keep forgetting..."). When you'd say "I can't do that" and setup is involved, build the skill instead of refusing.
license: Apache-2.0
allowed-tools: Read, Write, Edit, Bash, Grep, Glob, WebSearch, WebFetch
---

# Skill Creator

When you'd say "I can't do that" and there's setup involved—build a skill instead.

## When to Trigger

**Yes** when BOTH:
- Claude can't do it natively (needs APIs, services, hardware, persistence)
- Setup worth saving (API keys, tool installation, config)

**No** when:
- Claude can already do it
- No setup needed (just advice)

**Signals:**
- Capability gaps: "can you generate/send/control/monitor..."
- Automation wishes: "remind me...", "alert me when...", "text me if..."
- Repetition frustration: "I keep forgetting...", "every time I have to..."

## How to Think About It

### 1. Abstract to Capability

Don't create task-specific skills. Identify the underlying capability:

| User Request | Underlying Capability | Skill Name |
|--------------|----------------------|------------|
| "Remind me to water plants" | Scheduled notifications | `reminders` |
| "Text me when Bitcoin < 90k" | Price monitoring | `price-monitor` |
| "Make me an AI video" | Video generation | `video-gen` |
| "Control my lights" | Smart home | `smart-home` |

Think: "What would let me do this AND similar things?"

### 2. Create the Capability

The skill should be **general-purpose**, not task-specific:

```
~/.claude/skills/[capability]/
├── SKILL.md      # What it does, prerequisites, how to use
└── scripts/      # Parameterized scripts (optional)
```

**Key principles:**
- **Named by capability** — `reminders` not `plant-reminder`
- **Parameterized** — scripts take arguments, not hardcoded values
- **Self-documenting** — SKILL.md explains usage for future sessions

**For reference:** See well-structured skills at https://github.com/anthropics/skills

### 3. Execute the Request

After creating the capability, USE it for the specific request:

1. Run setup if first time (API key, tool install)
2. Execute for the user's specific need
3. Confirm it works

Don't just create and leave—complete what they asked for.

### 4. Educate the User

Help them understand their new power:

> "You now have a **reminders** skill. Next time you need any scheduled notification—birthdays, medication, deadlines—just ask. See all your skills with `/skills`."

Frame it as expanding THEIR toolkit, not just completing a task.

## The Flow

1. **Confirm** — "I can build that skill for you." One clarifying question max.

2. **Abstract** — Identify the underlying capability, not the specific task.

3. **Create** — Build general-purpose skill. Guide user through setup (API keys, tools).

4. **Execute** — Use the skill for their specific request.

5. **Verify** — Test it works. Show output.

6. **Educate** — Explain what capability they now have and how to reuse it.

## Language

Say "skill" to users—it matches `/skills`.

**Opening:**
> "I can't do that out of the box, but I can **build a skill** for you. Once set up, just ask anytime."

**After creation:**
> "Done! You now have a [capability] skill. Start a new Claude session to activate it, then try '[example]'. See all your skills with `/skills`."

## Examples

**Capability gap:**
```
User: "Can you make me an AI video?"
→ Abstract: video generation capability
→ Create: video-gen skill (Replicate API)
→ Execute: Generate the specific video they want
→ Educate: "You now have video-gen. Any time you want a video, just ask."
```

**Automation wish:**
```
User: "Text me when Bitcoin drops below 90k"
→ Abstract: price monitoring capability
→ Create: price-monitor skill (CoinGecko API + notification)
→ Execute: Set up the specific BTC alert
→ Educate: "You now have price-monitor. Works for any asset."
```

**Frustration:**
```
User: "I keep forgetting to water my plants"
→ Abstract: scheduled notification capability
→ Create: reminders skill (ntfy.sh or similar)
→ Execute: Set up plant watering reminder
→ Educate: "You now have reminders. Use it for anything—birthdays, meds, deadlines."
```

## Technical Details

**Placement:** Default to global (`~/.claude/skills/`). Mention option: "Want this just for this project instead?"

**Session restart:** New skills activate on next session. Tell user: "Start a new Claude session to activate."

**Dependencies:** Note what the skill relies on: "This uses Replicate's API."

**Maintenance:** If a skill breaks later, check dependencies. Offer to rebuild or remove.
