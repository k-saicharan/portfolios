# Sai Charan — agent skills (public pack)

Portable `SKILL.md` procedures others can install into an agent skills directory — same idea as public skill packs (gstack-style: one folder per skill, markdown as the interface).

**Free to use.** These files are stripped of personal context (employers, clients, home paths, email, private vault IDs, credentials, overly personal Board/Priorities content). What remains is the mechanism: when to run, cascade rules, and generic procedures. Rename placeholders like `your-vault` / `project folder` to match your stack.

## Skills in this pack

| Skill | What it does |
| --- | --- |
| **xenrich** | Closing the loop on bookmarks: stage → you fill Why → process → file → ledger → delete. |
| **log** | Session log per project so decisions, opens, and residue compound when the chat closes. |
| **autopilot** | On-demand execution layer: Board face, cascade, sweep fronts, stall irreversible sends. |
| **insight-radio** | Turn learning seeds into world-branched packs with real sources (NotebookLM-ready). |

## Install

### Option A — copy into Claude Code / compatible agent skills

```bash
# from this directory (or after unzipping saicharan-agent-skills.zip)
cp -R xenrich log autopilot insight-radio ~/.claude/skills/
# or into a project-local skills dir your agent already reads
```

Commands that originally lived as slash-commands (`/log`, `/autopilot`) are shipped here as skill folders with `SKILL.md` so they install the same way as xenrich / insight-radio.

### Option B — npx-style / skills CLI

If you use a skills installer that accepts a folder or zip of `*/SKILL.md` packs:

```bash
# example shape — use whatever installer you already run
npx skills add ./saicharan-agent-skills.zip
# or point it at this repo path / the unzipped skills/ directory
```

Exact CLI flags vary by tool; the contract is: each skill is a directory containing `SKILL.md` with YAML frontmatter (`name`, `description`).

## Browse

Open `index.html` in a browser for a light list + per-skill links, or read the markdown directly.

## Licence / intent

Use, adapt, fork. Attribution appreciated but not required. These are procedures, not a product — no warranty, no support surface.
