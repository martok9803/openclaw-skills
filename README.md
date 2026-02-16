# openclaw-skills

A public collection of **OpenClaw AgentSkills**.

The goal: small, composable “capability packs” that make an OpenClaw agent reliably good at a specific job.

## Skills

### reminder-engine
Natural-language reminders backed by OpenClaw cron jobs.

**What it does**
- Turns requests like:
  - “remind me in 20 minutes to stretch”
  - “tomorrow at 9 remind me to pay rent”
  - “every weekday at 10:30 remind me to stand up”
  into a cron job (one-shot or recurring).
- Supports reminder management:
  - “list my reminders”
  - “cancel reminder <id>”
  - “snooze <id> for 1 hour” (implementation guidance)

**Safety defaults**
- Always confirm the computed schedule before creating/removing jobs.
- Avoid secrets in reminder text.
- Warn before posting reminders into public channels.

## Install / Use

### Option A: use the packaged `.skill` file
Download the latest packaged skill from this repo:
- `dist/reminder-engine.skill`

Then install it into your OpenClaw workspace (common pattern):
- Place it under your workspace `skills/` directory (or import it using your preferred OpenClaw method).

### Option B: use this repo as a working directory
Clone the repo and copy/link the desired skill folder into your OpenClaw workspace:
- `skills/reminder-engine/`

## Development

Local validation + packaging (requires the OpenClaw `skill-creator` scripts):
- Validate: `quick_validate.py skills/reminder-engine`
- Package: `package_skill.py skills/reminder-engine ./dist`

## License

MIT.
