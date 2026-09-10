---
name: autopilot
description: Execute an on-demand Autopilot system — sweep non-creative fronts, reconcile trackers, surface blockers onto a single Board face with detail in a control-plane store.
registry: true
portable: true
---

# /autopilot — Execution layer

Execute the Autopilot system — sweep all non-creative fronts, reconcile trackers to current truth, surface blockers onto the Board (face) with detail in Autopilot.md (store).

---

## Key paths (adapt to your vault)

- **Board (face — read/write first):** `your-vault/Get Shit Done/Board.md`
- **Control plane:** `your-vault/Get Shit Done/Autopilot.md`
- **Priorities:** `your-vault/Get Shit Done/Priorities.md`
- **Window:** `your-vault/Get Shit Done/Window.md`
- **Systems face:** `your-vault/Systems/Home.md`
- **Vault root:** `your-vault/`
- **Execution folder:** `your-vault/Get Shit Done/`

---

## Step 0a — Board first (before anything else)

**Board is the single index for anything that needs the user.** They answer only under *Needs you* (`context:`), or at a location a Board row points to (e.g. xenrich Staging, note verdicts). Never treat Autopilot, Home, or STATUS as the answer surface. Autopilot has no fillable blanks; every detail block ends with "answer on Board."

Read `Board.md` in full.

0. **Machine dirty-check (before claiming untouched):**
   ```bash
   BOARD="your-vault/Get Shit Done/Board.md"
   # If Board file mtime is newer than frontmatter last-processed, user wrote since last ingest.
   # Parse last-processed from YAML; compare to mtime. If dirty → must ingest.
   # Never infer "untouched" from Autopilot.md alone.
   ```
   If `mtime(Board.md) > last-processed` (frontmatter), Board is dirty: ingest fully. **Do not open Autopilot looking for answers.**
1. **Ingest ticks:** any `- [x]` items → cascade into Priorities/Window/Autopilot ANSWERED, then **remove them from "Needs you"** (open section holds open rows only). A tick with empty `context:` is **not** an answer if the item required numbers/a decision; re-open or flag.
2. **Ingest `context:` lines** that are non-empty → act/unblock. Do not re-ask what was already answered.
3. **Write agent verdicts in the Run log**, not multi-paragraph blocks inside open Needs-you rows. After processing an answer: slim the open row to what is still theirs, or tick+move off if fully done.
4. **Update `last-processed`** (ISO timestamp) and re-derive frame (shift line, hard-date ages, `[open Nd]`) whenever you bump `updated:`.
5. Only after this pass: load Autopilot policy and sweep fronts.

If Board is stale vs Priorities, fix Board during this run. Board must not lie.

### Human-ask rule (every front)

If a skill or sweep needs the user: write **one Board row** under Needs you (tag + one-line ask + blank `context:`). If the answer must live elsewhere (bulk Why lines, per-note verdicts), the Board row **links that place**; they still own the row until ticked. Never ask only in STATUS, Home, chat, or Autopilot.

---

## Step 0 — Git snapshot (before touching anything)

```bash
VAULT="your-vault"
cd "$VAULT" && git add -A && git commit -m "autopilot: pre-run snapshot $(date +%Y-%m-%d\ %H:%M)" 2>/dev/null || echo "git snapshot skipped (not a repo or nothing to commit)"
```

---

## Step 1 — Idempotency check

Read the RUN LOG section at the bottom of `Autopilot.md`. Find the timestamp of the most recent run entry.

Then check the mtimes of the four key files (Board, Window, Priorities, Autopilot).

**If all four mtimes are older than the last run entry AND the run log says "nothing to move" or "no drift":** append one line to the RUN LOG — `- **<date> (run):** Nothing changed since last run — skipped.` — then stop. Do not re-sweep.

Otherwise proceed.

---

## Step 2 — Load the policy

Read `Autopilot.md` in full. The THE POLICY section governs what you may advance freely vs. what must stall to the queue. Load it now; it applies to every action from here forward.

Also read the SAFETY guardrails section. Hard limits on what you may write, delete, or modify.

---

## Operating principle — context depth governs every front

Before acting on any item, on any front, assess how much context exists:

- **Rich context** (tracker entry is specific, vault note has detail, recent activity is clear) → **act first.** Research, draft, produce the artifact. Show the result after.
- **Thin context** (vague note, missing data, no recent signal) → **ask one targeted question.** Get the answer, then act.

This is not an ideas-only rule. It applies equally to every front. Never ask permission for things you can derive. Never act blind on things you can't.

---

## Step 3 — Standing job: freshness + drift check

1. Read Window.md and Priorities.md in full.
2. Note when each was last modified (mtimes from Step 1).
3. Check for **drift**: does what the file claims match the timestamps? If Window was last edited 10 days ago, treat everything in it as potentially stale.
4. Check mail and calendar (via available tools) for signals the trackers may have missed — new confirmations, new deadlines, replies that changed status.
5. If you detect drift — a file's claim contradicts observed activity or recent mail — flag it. Don't silently trust the stale file. Queue a reconciliation question if needed.
6. Compute the current shift from the SHIFT CONTEXT section in Autopilot.md (deterministic — no need to ask). Update the shift field in Window.md if it's wrong.

---

## Step 4 — Sweep the fronts

Work through each front configured in your Priorities / Autopilot policy. For each one: read the relevant tracker section, check for deadline movement, check for available research or draft work you can advance. Apply the policy (advance freely vs. stall and ask).

Adapt the front list to your life. A typical non-creative set looks like:

### Front 1: Admin / compliance deadlines
- Check milestone state and deadlines within ~30 days.
- Advance: research, draft prep, document status updates in Priorities.md.
- Stall: anything requiring account access, a final submit, or spending.

### Front 2: Current-role internal growth
- Check learning plan / cert / internal path progress.
- Advance: research, path updates, tracking open actions.
- Stall: reaching out to a specific person (that's the user's voice).

### Front 3: Job hunt (parallel pipeline)
- Check pipeline state via your job-hunt tracker or Priorities.md.
- Surface deadlines and next actions.
- Advance: tracker updates, research on roles, draft prep.
- Stall: submitting applications, sending emails.

### Front 4: Finance
- Check open items (statements, upcoming payments, leak surfacing).
- Advance: tracker updates, math, surfacing issues.
- Stall: family or shared-money levers — always the user's call.

### Front 5: Idle built assets
- Side projects / prototypes already built — deploy/positioning status of each?
- Advance: research, deployment status tracking, positioning notes.
- Stall: anything requiring credentials or a live deployment decision.

### Front 6: Creative runway (non-judgment work only)
- Contest/festival calendar, free-entry deadlines, web presence tasks.
- Advance: deadline watch, calendar entries, research.
- Stall: anything requiring creative judgment or the user's voice.

### Front 7: Ideas bucket / vault handoff
- Check items in your vault Inbox or Void-equivalent log marked as needing autopilot attention.
- Scan the git snapshot diff from Step 0: any new vault notes this run with a non-creative actionable signal (a tool to buy, a practice to start, a device to research)? Research and write findings **back into the origin note** under a labelled section. Do not route to Priorities by default.
- Surface these in the Step 10 report. If research requires judgment first, ask one question in the report — not as a Board flood.
- Stall: anything ambiguous — drop it in the queue with context.

---

## Step 5 — Apply the cascade rule

For every update you make: propagate to **every core file the update touches**, not just the nearest one. An update to a deadline in Priorities should also hit Window if Window references it. An answered question in the DECISIONS queue should flow to every tracker it affects.

Shallow propagation (one file) is the failure mode. Don't do it.

---

## Step 6 — Save artifacts to the vault

Any artifact produced during this run — research brief, comparison, draft, recommendation, feasibility note — must be written to the vault before the session ends. It does not live in the chat window.

**Where artifacts go (within the write allowlist):**
- Research that enriches a specific vault note → **append to that origin note** under a clearly labelled section.
- Front-specific drafts → append to that front's `log.md` or save in its folder.
- Genuinely new standalone artifacts with no origin note → a working-notes folder in your vault.
- Do not default to the execution folder for things that clearly belong to an existing note.

**Run log must reference it:** the RUN LOG entry includes the file path of every artifact saved.

---

## Step 7 — Write blockers to Board (face) + Autopilot detail (store)

For every item that stalls and needs the user:

1. **Board first** — add or update one checkbox line under `## Needs you` in `Board.md`:
   ```
   - [ ] **[front]** **short ask.** one line of stakes. → [[link]]
       - context:
   ```
   Cap Board at ~10 open items. Overflow collapses into one Board line pointing at Autopilot detail.
   If the item creates a real cross-front link, add one line to a Weaves section. If it is a hard date, add one line to Hard dates. Do not invent weaves for everything.

2. **Autopilot detail** — add/update the matching block under `## DECISIONS FOR YOU` in `Autopilot.md`:
   ```
   ### [front] short question
   Why it's stalled: one line.
   Options if any: a / b / c.
   **Your call:** ____
   ```

3. **Systems/Home.md** — if this run changed a skill queue count (xenrich staging, radio seeds, inbox), bump the relevant row + `updated:` date. Do not dump life tasks onto Home.

Be surgical. One question per real blocker. Board is the face; Autopilot is the store.

---

## Step 8 — Git snapshot (after)

```bash
VAULT="your-vault"
cd "$VAULT" && git add -A && git commit -m "autopilot: post-run snapshot $(date +%Y-%m-%d\ %H:%M)" 2>/dev/null || echo "git snapshot skipped"
```

---

## Step 9 — Append to RUN LOG

Append one terse, dated line to the `## RUN LOG` section of `Autopilot.md`:

```
- **<YYYY-MM-DD> (run):** <what moved> / <what stalled> / <# of questions queued>.
```

Keep it to one line. The log is a pulse, not a report.

---

## Step 10 — Report back

Tell the user: what moved on each front (or "nothing to move"), how many open items are on **Board**, and one sentence on anything urgent. Point them at Board, not Autopilot, for answering. No padding.

If Front 7 produced actionable vault signals this run: list them and the origin files updated. Keep the confirmation loop to a single exchange.
