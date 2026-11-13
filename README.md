# Haven

**A Discord-based AI assistant I built to use on my Windows computer.**

I wanted one place to ask for help with everyday work, from checking my calendar to comparing contact lists. I built Haven so it could carry out those requests through Discord and show me what happened. It runs on my computer and uses Claude with Codex as a backup.

## What I built

| Feature | How it works |
| --- | --- |
| **AI failover** | Haven can route a request to Codex when Claude is unavailable and carry recent conversation context across the switch. It checks for actions that may already have run before deciding whether to retry. |
| **Shared browser** | Both AI backends use the same Playwright-controlled Chrome profile. They can open pages, read and interact with them, and return screenshots in Discord. |
| **Approval cards** | Before certain actions, such as sending email or changing a calendar event, Haven shows me what it plans to do. The action runs only after I approve it. |
| **CRM and automations** | A SQLite-backed dashboard manages contacts, reviews likely duplicates, and compares records from two lists. A scheduler runs jobs such as reconciliation and health reports. |
| **Supervised updates** | Haven can prepare code changes in an isolated Git worktree and test them there. A separate supervisor starts the updated bot, checks that it connects to Discord, and can roll back a failed startup. |

## Example: finding a duplicate contact

| Fictional record | Company | Phone | Email |
| --- | --- | --- | --- |
| CRM | Cedar & Finch Studio | 304-555-0142 | hello@example.com |
| Partner list | Cedar and Finch Studio LLC | 304-555-0142 | studio@example.com |

Haven normalizes the company names and recognizes the shared phone number. It flags a likely match and puts the two email addresses side by side for review. I decide which details to keep.

## A bug I tracked down

Codex used to say it couldn’t control the browser even though Haven had already registered the browser tools. Restarting Haven brought up a new bot process, but Codex resumed a thread with older instructions. The updated session handling starts a fresh AI session after a build or instruction change and carries over recent exchanges.

I verified the fix from Discord. Codex opened a test page, posted a screenshot, and closed the test tab. I then restarted Haven from Discord and repeated the browser check. I also denied a test calendar approval and confirmed the create action was never run.

## Built with

Python, discord.py, Claude Agent SDK, Codex SDK, Playwright, SQLite, Starlette, Docker, and Git.

This repository is a portfolio overview. The running assistant and its source live in a private repository.
