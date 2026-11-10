---
name: hackathon
description: Hackathon mode for pre-launch projects with no users. Ship straight to main and prod without asking, and skip migration, data-loss, legal-versioning and refactor caution. Load it when the user types /hackathon, or when the project's CLAUDE.md or AGENTS.md has a "Hackathon mode: ON" block. Never apply it to a project that has neither. `/hackathon install` turns it on for the current project and `/hackathon off` turns it off.
argument-hint: "[install | off]"
---

# Hackathon mode

> ⚠️ **Fair warning:** this skill removes the safety habits that protect real users. Use it only while **nobody outside the team uses the product**. **When you start onboarding users (beta testers, a first customer, a public launch), run `/hackathon off` or delete this skill.** From then on, migrations, data loss, policy changes and deploys matter again.

## Arguments

- **`install`** (or no argument when the project has no marker yet): add the marker block below to the project's `CLAUDE.md`. If the project has only an `AGENTS.md`, add it there. If it has neither, create `CLAUDE.md`. Then commit and push it like any other change, and show the user the fair warning above, word for word.
- **`off`**: remove the marker block, commit and push, and tell the user that normal caution is back. Stop following this skill for the rest of the session.
- **The marker is already present**: just follow the rules below.

Marker block to insert:

```markdown
## Hackathon mode: ON

No users yet. Claude follows the `hackathon` skill: commit and push to main, deploy to prod, and don't ask.
⚠️ Remove this block (`/hackathon off`) before onboarding the first user. Once it is gone, normal caution applies.
```

## The premise

There are no customers and no real user data. Production is a demo environment that happens to be public. Breaking it costs minutes, not trust. So speed beats caution: act, ship, verify, fix forward.

## Rules

### Git and deploy

- After every change that builds or passes its checks, **commit and push straight to `main`**. Don't create feature branches or PRs, and don't ask "should I push?".
- **Deploy to prod every time.** If CI deploys on push (look in `.github/workflows/`), watch the run (`gh run list` / `gh run watch`) and don't also deploy by hand. Otherwise, deploy with the project's documented command.
- After a deploy, check that prod is actually up: a health URL, a page load or the logs. If it broke, fix forward right away, or roll back to the previous image or commit.
- Write commit messages normally. Small, frequent commits are fine.

### Databases and data

- Don't worry about migrations. Write them, and apply them automatically on deploy (prefer running pending migrations at container or app start).
- Destructive migrations are fine: dropping columns or tables, renaming, changing types, resetting the database, reseeding. No backfills, no multi-step expand/contract, no backups first.
- Use non-interactive migration tooling (for example `prisma migrate diff ... --script` plus `migrate deploy` instead of `migrate dev`).

### Terms, privacy and policies

- Edit Terms, Privacy Policy, DPA, cookie policy, pricing and marketing copy directly and ship them live. You don't need version history, "last updated" notices, re-consent flows, user notification emails or grandfathering.
- Price and markup changes go live without asking.

### Refactors and code

- Refactor freely: rename, move, delete dead code, change APIs and schemas, swap libraries.
- Skip backward-compatibility shims, deprecation periods, feature flags and dual-write phases. Nobody depends on the old shape.
- Still run the build, typecheck and tests before pushing. Speed comes from not asking, not from shipping broken code.

### Working style

- **Don't ask for permission** on anything this skill covers. Decide, do it, and say what you did.
- In long or unattended sessions, go as far as you can. When something blocks you (a missing credential, an action auto mode won't allow, a decision only the user can make), note it, move on to the next thing, and **list every blocker at the end**. Give each one a ready-to-run command for the user.

## What still matters (no users ≠ no consequences)

These are not about users, so hackathon mode does not relax them:

- **Secrets:** never commit, print or log secrets. Read them from the team's secret store (such as 1Password), write them without echoing them, and keep `.env` files gitignored.
- **Read before you replace config:** some cloud CLIs replace the whole env or secret set on update (for example `scw container container update`). List the current keys first, send the complete set, and check it afterwards. A partial update took a prod environment down once.
- **Other people's stuff:** don't force-push or rewrite `main`'s history (teammates' work), don't email, message or charge real third parties, and don't scan or attack systems the team doesn't own.
- **Money:** watch out for anything that can run up uncapped cloud or API spend (loops, autoscaling, LLM calls without a budget).
- **Credentials and IAM:** creating keys or widening access may be blocked by auto mode. Hand the user the exact command instead of working around the block.

## When to stop using this skill

As soon as any of these is true, tell the user and suggest `/hackathon off`:

- A real user or customer signs up, or a beta or launch is announced.
- Real personal or customer data lands in the prod database.
- Payments go live with real money.
