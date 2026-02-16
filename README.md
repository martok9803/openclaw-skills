# openclaw-skills

Open-source **OpenClaw AgentSkills**.

This repo is a small “skill pack” library: each skill is a focused capability with:
- a clear trigger description
- a safe workflow (confirmation gates where needed)
- optional scripts for deterministic tool-backed execution

## Skills

- **reminder-engine** — reliable reminders backed by OpenClaw cron jobs.
  - Source: `skills/reminder-engine/`
  - Package: `dist/reminder-engine.skill`
  - ClawHub: `martok9803-reminder-engine`

- **ci-whisperer** — GitHub Actions autopsy + (optional) PR fix mode.
  - Source: `skills/ci-whisperer/`
  - Package: `dist/ci-whisperer.skill`
  - ClawHub: `martok9803-ci-whisperer`

## Install

### Install from ClawHub (recommended)

```bash
npx --yes clawhub@latest install <skill-slug>
```

Examples:
```bash
npx --yes clawhub@latest install martok9803-reminder-engine
npx --yes clawhub@latest install martok9803-ci-whisperer
```

### Manual install (workspace)
Copy a skill folder into your OpenClaw workspace:
- `<your-openclaw-workspace>/skills/<skill-name>/SKILL.md`

## Proof / Quick tests

### reminder-engine (2 minutes)
1) Ask your agent: “remind me in 1 minute to test reminders”
2) Confirm the computed schedule.
3) Verify:
   - `cron.list` shows the job
   - `cron.runs <jobId>` shows status `ok` after it fires

### ci-whisperer (5 minutes)
This repo includes a public demo repo that intentionally fails CI:
- https://github.com/martok9803/ci-whisperer-demo

1) List runs:
```bash
python3 skills/ci-whisperer/scripts/ci_autopsy.py list --repo martok9803/ci-whisperer-demo
```
2) Pick a run id and fetch failed logs:
```bash
python3 skills/ci-whisperer/scripts/ci_autopsy.py failed-logs --repo martok9803/ci-whisperer-demo --run-id <id>
```

## Safety philosophy

- Skills must not leak secrets.
- “Write actions” (PRs, deletions, posting) must be **opt-in** and require explicit approval.
- Prefer small, boring, debuggable scripts over clever magic.

## License

MIT.
