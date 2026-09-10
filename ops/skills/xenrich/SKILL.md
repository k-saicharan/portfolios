---
name: xenrich
description: |
  Bookmark enrichment in two phases. Phase 1 (stage): fetches bookmarks from topic
  folders plus the unfoldered queue, reads each linked resource fully, and writes a
  per-folder staging note in your vault with a blank Why line per item — no filing,
  no deletion yet. Phase 2 (process): reads whatever Why lines have been filled in,
  connects the answer to active life threads, and files the result (vault note,
  per-project Signals.md line, task, or lead) before deleting the bookmark. Activate
  Phase 1 on "/xenrich", "check my bookmarks", "stage my bookmarks". Activate Phase 2
  on "/xenrich process", "process the staged bookmarks", "xenrich is ready".
registry: true
portable: true
---

# /xenrich — Bookmark Enrichment

**Pipeline face:** `Systems/Xenrich/Xenrich Board.md` (or equivalent in your vault). Read it first on every run (Phase 1 or Phase 2). Rewrite its counts in the same pass as Staging. It is the cascade map, not a second inbox: Why lines stay in `Staging/`; the human index stays on your main Board. Same split as any domain board vs the single Board face.

You bookmark posts as a signal: "this matters somehow." The skill closes that loop — folder as filter, Why as enrichment, ledger as trail after the bookmark is gone.

## The design

Folder = which world an item belongs to, chosen by you at save time. That's a **filter, not an instruction** — it narrows which question to ask and which destinations make sense, it does not tell the skill what to do on its own. The actual enrichment — why you saved this specific post, what struck you about it — still has to come from you per item. The folder just scopes that conversation instead of running it cold.

Example folder taxonomy (adapt names/IDs to your bookmark tool):
- **Job** — role leads AND hiring/career-approach content; only unambiguous role postings skip the Why entirely
- **Creative** — craft, narrative, industry signal
- **AI & Tech** — tools, industry shifts, timing conviction
- **Business Ideas** — candidate ideas for your ideas bucket
- **Everything Else** — deliberate call that something doesn't fit the first four; still a categorization decision
- **Unfoldered** — anything bookmarked but not placed in a folder. NOT an error and NOT the same as "Everything Else." On many platforms, bookmarking lands here by default; adding to a folder is an extra step. Unfoldered means no categorization decision was made. Always check for these.

Every folder is a live to-process queue, not an archive. Processing always ends in deletion: either the source link traveled into something filed (vault note, task, lead), or the item was dismissed. Whatever is still sitting in a folder is, by construction, everything not yet processed.

**The ledger (`Systems/Xenrich/ledger.md`).** Deleting the bookmark removes the browsable list of "stuff I saved." The ledger replaces it: an append-only file where EVERY processed item lands one line — kept or dismissed — with its source link and where it went. Name it *ledger*, not *log* — `log.md` in the same folder is the session log; this is an index/register. It is an index, not a store — real content lives at its filed destination; the ledger guarantees the link and a trail always exist in one findable place.

## Two phases, one flow

Bookmark-saving happens on the platform; the thought about *why* it mattered arrives whenever it arrives. Forcing both into one synchronous pass degrades into "I don't remember." So xenrich runs in two phases:

- **Phase 1 — Stage** (`/xenrich`, "check my bookmarks", "stage my bookmarks"): fetch, read, triage, write a staging note per folder with a blank `Why:` line per item. No filing, no deletion.
- **You fill in the Why lines** — directly in the staging note, whatever length, whenever the thought is there.
- **Phase 2 — Process** (`/xenrich process`, "process the staged bookmarks", "xenrich is ready"): read whatever's filled in, enrich, file, delete the bookmark, log the ledger line. Items still blank stay staged — never nudged, never auto-dismissed.

Staging notes live in `Systems/Xenrich/Staging/`, one file per folder (`<folder>--<date>.md`). If a staging file already exists from an unfinished stage, Phase 1 appends — it never clobbers whys already written.

## Phase 1 — Stage

1. **Fetch** — use your bookmark API / CLI (authenticated as your account). Prefer a reliable CLI path over a flaky MCP when both exist.
   - Per folder: list bookmark IDs in that folder.
   - Full bookmark list (to diff for Unfoldered): fetch recent bookmarks with text/entities where the API allows.
   - Diff full-list IDs against the union of folder IDs — remainder is **Unfoldered**.
2. **Scope Unfoldered by recency before processing.** Unfoldered can contain years of pre-system bookmarks. Pull timestamps and look for the natural gap (recent cluster, then a jump back). Confirm scope with the user if ambiguous — don't silently process a multi-year tail.
3. **Read each item fully — no skimming the preview.**
   - If the post contains a URL, fetch and read the actual page before staging.
   - For platform-native long-form: prefer the API body over a browser login wall.
   - Quote-posts / replies: pull the referenced post too so full context is in hand.
4. **Triage before staging.**
   - **Low-signal / dismiss-by-default**: pure reaction/hype with no connective tissue to an active thread. Single compressed staging line; no blank Why — user overrides in one word if they disagree.
   - **Worth the question**: anything that plausibly connects to an active thread. Full staging block with blank `Why:`.
   - When unsure, default to the full block.
5. **Job-folder fast path only:** unambiguous role postings (title, apply-by, requirements, "we're hiring") skip Why — route into your job-hunt pipeline, file, ledger, delete. Hiring-trend / career-approach content still stages.
6. **Write the staging note** — `Systems/Xenrich/Staging/<folder>--<date>.md`.
   - Worth-the-question: link/title, one sharp sentence grounded in full content (shaped by folder world), blank `Why:` line.
   - Low-signal: compressed line, no Why.
   - Unfoldered: fully open — ask both which world AND why.
7. **Output** — per folder, counts only (staged N, auto-dismissed M). Close with reminder that Phase 2 runs when the user says so.
8. **Board index (mandatory if any Why blanks remain):** one row on your main Board under *Needs you*:
   - `- [ ] **[xenrich]** N Why blanks in [[Staging file]] — fill there, then tick this row.`
   - Blank `context:` on the Board row.
   - Do **not** ask only in STATUS or chat. Board is the single index; Staging is where bulk Why lines are filled.
   - Rewrite `Xenrich Board.md` in the same pass.

## Phase 2 — Process

1. **Read the staging notes** — everything in `Systems/Xenrich/Staging/`.
1a. **Recurrence check — BEFORE deciding any destination.** Per-item routing shreds a subject that keeps coming back across destinations.
   - Search the ledger for the item's subject, product, or author before filing.
   - If a folder already exists for that subject: route there. Check its STATUS first.
   - If this is the 3rd-or-later hit and no folder exists: stop and surface it. User decides — never auto-create.
   - Distinguish an object (product/tool/company) from a thesis (usually a section inside an existing folder).
   - If a folder does get opened, collect scattered prior mentions in the same pass.
   - Two hits is not a thread. Note it, don't act.
2. **For each item with a filled `Why:` line — enrich, don't just log the answer:**
   - Connect the reason to what's already active (job pipeline, creative threads, tech timing, ideas bucket, current work context) — say what this connects to.
   - Decide the actionable shape: vault note / task / lead / follow-up / research deeper / consciously park / nothing needed. Every shape except "nothing needed" results in something filed.
     - **Existing project/idea folder**: append ONE line to that project's `Signals.md` — never the main strategy doc. Format: `- [date] [title](link) — one-line why-it-matters. Status: unevaluated.`
     - **Nothing existing matches**: one-line task on your execution board (not an orphaned vault catch-all).
   - Full vault notes only for things that warrant real synthesis; live with the idea's own folder.
   - Respect hard-skip zones in your vault (journal, dreams, private creative drafts) — surface findings instead of auto-writing.
   - **Bridge to Insight Radio:** if dense/public learn-worthy material, drop an Insight Radio seed tagged with its **world** (same taxonomy). Private-strategy items skip.
   - **Bridge to a "To Know" queue:** if the Why is a person or work to check, leave a thin capture card (name, where, must-check) — not a biography.
   - **File** with the source link included so the trail survives bookmark deletion.
   - **Record in the ledger** (before deletion):
     `- [YYYY-MM-DD] [title](link) — [folder] → landed: <destination> | <one-line why / disposition>`
   - **Delete the bookmark** once filed and logged.
   - **Remove that item's block from the staging note.**
3. **Blank items** — leave exactly as staged. Don't nudge.
4. **Low-signal lines** — dismiss per default unless user wrote an override; ledger + delete.
5. **Housekeeping** — delete empty staging files.
6. **Output** — two to three lines per processed item (what / which thread / where it landed). Close with what's still staged per folder.
7. **Board index after Phase 2:** clear or update the `[xenrich]` Board row; rewrite `Xenrich Board.md`.

## Tone

Not a summary machine. You're a thinking partner reading what caught their attention — full source, not the preview. The folder tells you which world to think in — it doesn't answer "why did YOU save this, and what does it want to become?"

If a folder is empty, say so plainly and move on.
