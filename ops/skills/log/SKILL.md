---
name: log
description: Write a session log entry for the current project. Captures voice, decisions, opens, and residue so continuity compounds when the chat closes.
registry: true
portable: true
---

# /log — Session log

Write a session log entry for the current project.

---

## What this skill does

Captures the current session — the user's voice, decisions made, things found, things opened — and appends it to the project's log file. If no log file exists, creates one. Works on any project: code, creative, vault notes, research.

---

## Step 1 — Find the project

Determine the active project from context:

- If working in a code repository: the project root (where `.git` or project instructions live)
- If working on a vault/creative project: the project folder referenced in this session (e.g. `your-vault/Projects/[ProjectName]/`)
- If working on something without a clear folder: the primary file or directory that was the focus of the session
- If ambiguous or no project can be identified (meta-conversations, workflow discussions, one-off explorations): use a global session log at `your-vault/Sessions/log.md` — do not ask

## Step 2 — Find or create the project folder and log file

**First, establish a project folder.** The log must never be an orphan file floating in a parent directory.

- If the project already has a dedicated subfolder: use it as the home.
- If the project is currently a single `.md` file in a parent directory: create a subfolder named after that file (same name, no `.md` extension), move the original file into it, and treat that subfolder as the project home. No orphans — both the project file and `log.md` live inside the folder.
- If the project is a code repo or has a clear root folder already: use that root as the home.

**Then, look for `log.md` inside that project folder.**

If it exists: read the existing structure so the new entry matches the established format.

If it doesn't exist: create it inside the project folder with this header:

```
# [Project Name] — Session Log
*Raw record. Not organised. Not polished. Preserved as-is.*

---
```

## Research & session output filing rule

When a session produces research, analysis, or meaningful output on a specific idea or project:

- File it **inside that idea's own folder** (e.g. `project-folder/Research.md` or `log.md`).
- Never dump research into a generic catch-all research folder — it becomes untraceable.
- If the idea has no folder yet, create one under your ideas/projects path and place the output inside it.
- This protects the time invested in the session. Research always travels with the idea it belongs to.

Update the log entry to reference any new research file created inside the project folder.

## Step 3 — Write the session entry

Append a new session block to the log. Format:

```
## Session [N] — [YYYY-MM-DD]

---

[Session content — see rules below]

---

*Log continues in future sessions.*
```

If a previous session already ends with `*Log continues in future sessions.*`, remove that line from the previous entry and place it only at the end of the new one.

**Rules for session content:**

- **Voice:** Write in the user's voice where they spoke directly. Use blockquotes (`> `) for their actual words — transcribed, not paraphrased. Fill in the connective tissue between their statements so the entries make sense standalone without being a Q&A transcript.
- **Capture decisions, not process:** What was decided, what was locked, what was discarded. Not every message exchanged — the residue that matters.
- **Capture what was opened:** Questions that came up but weren't answered. Directions that were named but not pursued. These matter as much as what was resolved.
- **Capture outputs:** Files created, files changed, delegations made, tasks written. Name them.
- **If the session was exploratory:** capture what territory was covered and what the session landed on, even if nothing was decided.
- **Length:** Match the density of the session. A short session gets a short entry. A long session gets a full record. Don't pad, don't compress beyond recognition.

## Step 4 — Report back

Tell the user: the log was updated (or created), the file path, and one sentence on what this session's entry captured.
