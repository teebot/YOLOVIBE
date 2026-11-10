# YOLOVIBE

A [Claude Code](https://claude.com/claude-code) skill for hackathon mode: commit and push to `main`, deploy to prod, and don't ask. While a project has no users, it also skips caution about migrations, data loss, policy versioning and refactors.

> ⚠️ **Fair warning:** this skill removes the safety habits that protect real users. Use it only while **nobody outside the team uses the product**. **When you start onboarding users (beta testers, a first customer, a public launch), run `/hackathon off` or delete this skill.**

## Install

```sh
git clone https://github.com/teebot/YOLOVIBE.git /tmp/yolovibe
mkdir -p ~/.claude/skills && cp -r /tmp/yolovibe/skills/hackathon ~/.claude/skills/
```

The skill is then available in every project, but it only applies where you turn it on.

## Use

- `/hackathon install` turns hackathon mode on for the current project. It adds a "Hackathon mode: ON" block to the project's `CLAUDE.md` (or `AGENTS.md`), so later sessions keep following it.
- `/hackathon off` removes the block, and normal caution comes back.

## What it relaxes

- **Shipping:** push straight to `main` and deploy to prod, then verify prod is up and fix forward if it broke.
- **Databases:** migrations run on deploy, and destructive migrations or resets are fine.
- **Policies:** Terms, Privacy, DPA, pricing and site copy ship live with no versioning or re-consent.
- **Refactors:** free, with no backward-compatibility shims or feature flags.
- **Working style:** no permission questions. Long sessions go as far as they can and list blockers at the end.

## What it keeps

Never commit or print secrets. Read cloud config before replacing it. Don't force-push `main`. Don't touch third parties or systems you don't own. Watch for uncapped spend.

See [`skills/hackathon/SKILL.md`](skills/hackathon/SKILL.md) for the full rules.
