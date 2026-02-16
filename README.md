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
Download:
- `dist/reminder-engine.skill`

Install into your OpenClaw workspace using your preferred method.

### Option B: use this repo as a working directory
Clone the repo and copy/link this folder into your OpenClaw workspace:
- `skills/reminder-engine/`

## Proof / Quick test (2 minutes)

This skill uses OpenClaw’s built-in **cron** scheduler. The simplest proof is to create a 1–2 minute reminder and verify it ran.

1) Ask your OpenClaw agent:
- “remind me in 1 minute to test reminders”

2) The agent should respond with the interpreted schedule and ask for confirmation.

3) After confirmation, verify the job exists:
- `cron.list` (you should see a new job with a next run time)

4) Wait ~1 minute, then verify it fired:
- `cron.runs <jobId>` (status should be `ok`)

5) Optional cleanup:
- “cancel reminder <jobId>” (or `cron.remove`)

### How it works (under the hood)

When you ask for a reminder, the agent creates a cron job:
- One-shot reminders → `schedule.kind = "at"`
- Recurring reminders → `schedule.kind = "cron"` (with timezone if supported)

And the payload is a `systemEvent` that reads like a reminder when it fires.

## Development

Local validation + packaging (requires the OpenClaw `skill-creator` scripts):
- Validate: `quick_validate.py skills/reminder-engine`
- Package: `package_skill.py skills/reminder-engine ./dist`

## License

MIT.
