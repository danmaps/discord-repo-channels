# Discord Repo Channel Helper

A tiny, repo‑portable pattern for creating *structured conversations* around issues, PRs, files, and actions — with optional automation hooks to create a per‑repo Discord channel.

## Why
Teams often start a repo and then scramble to organize conversation. This provides a lightweight, repeatable format you can drop into any repo so that:

- every repo has a clear place for **decisions**, **open questions**, and **next actions**
- conversations are **structured but flexible** (works in GitHub, Discord, or both)
- you can **automate** channel creation without committing to a heavy bot

## Quick Start

1. Copy this file into your repo (or add via template).
2. Fill in the **Repo Metadata** section.
3. Use the **Conversation Blocks** to keep discussions tidy.
4. (Optional) Add the webhook automation to auto‑create a Discord channel.

---

# Repo Metadata

- **Repo:**
- **Discord server (guild) id:**
- **Default channel name:** `#proj-<repo>`
- **Category:** `Projects` (or your preferred category)
- **Owners:**

---

# Conversation Blocks

Use these in issues/PRs, or in the Discord channel topic/pins.

## 1) Issue / Task Thread

```
## Context
- What is the problem?
- Why now?

## Goals
- What “done” looks like

## Constraints
- Time, risk, dependencies

## Options
- A, B, C

## Decision
- What we picked and why

## Next Actions
- [ ] Action item
```

## 2) PR Review Thread

```
## Summary
- What changed

## Risks
- Edge cases or regressions

## Tests
- What was run

## Reviewer Focus
- What to pay extra attention to
```

## 3) File / Design Discussion

```
## File(s)
- path/to/file

## Intent
- What the file should do

## Proposed changes
- Bullet list

## Open questions
- ?
```

## 4) Action Log (fast, lightweight)

```
## Decisions
- 

## Open Questions
- 

## Next Actions
- [ ]
```

---

# Automation (Optional)

## Minimal Webhook Service

**Goal:** when a repo is created (or added to a list), create a Discord channel automatically.

**Inputs**
- Repo name
- Discord guild id
- Category id (optional)

**Output**
- New text channel (e.g., `proj-<repo>`)

### Example Pseudocode

```js
// on webhook (repo created)
const name = `proj-${repo}`;
await discord.createChannel({
  guildId,
  name,
  type: 0, // text
  parentId: categoryId
});
```

### GitHub → Webhook → Discord

- GitHub webhook event: `repository.created`
- Webhook target: small service or serverless function
- Discord API: `POST /guilds/{guild.id}/channels`

---

# Helper Functions (Language‑agnostic)

Use these helpers in whatever stack you prefer:

- `normalizeRepoName(repo)` → `proj-<repo>`
- `ensureCategory(guildId, name)` → category id
- `ensureChannel(guildId, channelName, categoryId)` → channel id
- `postStarterMessage(channelId, template)` → pin starter

---

# Templates (Starter Messages)

**Channel topic**
```
Repo: <name> | Owner: <name> | Goals: <short>
```

**Pinned starter**
```
Use /issue, /pr, /design to keep discussion structured.
```
