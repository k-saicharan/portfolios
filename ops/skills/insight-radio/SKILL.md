---
name: insight-radio
description: |
  Build an Insight Radio audio pack (for NotebookLM or later own-voice TTS) from
  accumulated learning seeds, branched by world (Job/Creative/AI & Tech/Business
  Ideas/Everything Else — same taxonomy as xenrich) so unrelated topics never
  collapse into one file. One command: collect + tag seeds → gate for
  weave-worthiness → cluster per world → find/lock REAL sources → consolidate →
  weave (length-capped) → verify → assemble the 3-file pack per world → housekeep.
  Insight Radio is a curation-for-LEARNING tool: the weave teaches on the ideas'
  own merit and NEVER pattern-matches to the user's projects. Activate on
  "/insight-radio", "build an insight radio pack", "run insight radio",
  "process the radio seeds".
registry: true
portable: true
---

# /insight-radio — Build a learning pack from seeds

Insight Radio is a **curation-for-learning tool**, not a summariser. Its guarantee: *"the pointing was the product"* — every load-bearing claim in a pack survives a click to a **real source**. This command is the **consumption side** of a standing intake: other skills drop seeds, this turns accumulated seeds into a pack. Read your system's Insight Radio design note / log before running.

Vault paths (adapt `VAULT` to your vault root):
- Home: `VAULT/Systems/Insight Radio/`
- Intake contract: `VAULT/Systems/Insight Radio/Inbox/README.md`
- Drop-folder (standalone seeds): `VAULT/Systems/Insight Radio/Inbox/`
- Output packs: `VAULT/Systems/Insight Radio/PACK-<world>-<date>/`
- Archive of consumed packs: `VAULT/Systems/Insight Radio/Archive/`

## THE THREE NON-NEGOTIABLE RULES

1. **Learning, not project-mirror.** The weave stands on the ideas' own merit. NEVER map material onto what the user is building. "This helps your X" is a miss — it reduces new understanding to confirmation of an existing lever.
2. **Real sources only.** A pack is built from verified, clickable master material, never from the agent's own interpretation dressed as fact. If a seed names threads but no real source is found/locked for one, that thread is a GAP the pack declares, not something invented.
3. **Never mix worlds in one pack.** A Creative-public seed and an AI & Tech seed never get woven together, even if both are ready the same run. Separate worlds, separate packs.

## Branch by world, not one undifferentiated queue

Every seed carries (or inherits) a **world** — the same taxonomy `/xenrich` uses (Job / Creative / AI & Tech / Business Ideas / Everything Else). Gate, cluster, and weave **per world**. Output path becomes `PACK-<world-slug>-<date>/`, not one global `PACK-<date>/`. A world with too few or too-thin seeds waits — never forced into a pack this run, and never merged into a different world's pack to make up the numbers.

## The three feeds (standing intake, not an autonomous daemon)

Seeds accumulate harmlessly until this command runs. Two drop mechanisms:

- **In-note marker (preferred):** a `→ Insight Radio:` line inside a vault note, carrying a world + open question + candidate threads + why. Emitted by any skill that finds dense/learnable material with a natural vault home.
- **Standalone drop file (fallback):** `Inbox/seed--<YYYY-MM-DD>--<slug>.md` for seeds with no home note (hand-drops, bare links).

Typical feeders: (1) idea-incubator / void-style value-add tails, (2) existing vault open-question notes, (3) `/xenrich` learn-worthy items, (4) a **To Know** queue of people/works to check.

### To Know episodes (one person or work per pack)

A **gathered** card (sources already locked, or ready to lock) is a demand-driven radio episode: know this person / this work, on their own terms.

- **Unit:** one person or one work = one pack. Never weave two people together. Never fold a To Know card into a world-bundle just to make up seed counts.
- **Cadence:** one gathered card is enough. Do not wait for 3+ seeds. Do **not** auto-pack on capture. Run when the user asks, or offer when a card hits `gathered`/`ready`.
- **Output path:** `PACK-toknow-<slug>-<date>/` so it cannot collide with a world bundle.
- **Don't stack.** If a prior `PACK-toknow-*` is still unconsumed, hold.
- **Same three non-negotiables.** The card's `world:` is metadata, not a license to mix with that world's other seeds.

## When to run (cadence — quality over pile-up)

Event-driven, never a scheduled/autonomous trigger. Run per-world against an explicit threshold: a world is **ready** when it has ~3+ dense, gated seeds, or its oldest seed has sat 2–3 weeks, whichever comes first. Surface this proactively (e.g. at the end of an `/xenrich` run) rather than leaving the user to track seed counts. **Exception: To Know episodes** — one gathered card is enough.

**Don't stack unconsumed packs.** Before starting a new pack in a world (or a new To Know episode), check whether that world's last pack has actually been listened to/read yet. If not, say so and ask whether to hold.

## Steps

1. **Collect + tag seeds.** Gather all pending seeds:
   - Match ONLY real marker lines (marker at line start, optionally bolded), never prose mentions: `grep -rnE "^\*{0,2}→ Insight Radio:" VAULT` — exclude backtick-quoted mentions and the consumed form `→ Insight Radio [packed …]:`. Also skip the `Systems/Insight Radio/` folder itself.
   - Read every `Inbox/seed--*.md`.
   - If invoked right after `/xenrich`, include items flagged learn-worthy (already world-tagged).
   - **To Know cards:** list cards with `status: gathered` or `status: ready`, plus any card that already carries `→ Insight Radio:`. List separately from world-bundles. Skip `captured` (too thin) and `checked`/`packed`.
   - Every seed must end up tagged with a world before step 2. If a seed predates the world field, infer it from content.
   Present the collected seed list grouped by world, 1 line each. List To Know candidates in their own block. If zero seeds and zero To Know candidates, say so and stop.

2. **Gate for weave-worthiness, then cluster per world.**
   - **Dense / learnable / public-discourse** seeds → keep for a pack.
   - **Private-strategy** seeds (immigration status, employer-internal clocks, personal licensing/finances, private decisions) → do NOT force a curation weave; mark them for a parked synthesis-first non-creative branch instead, and say so.
   - Among what's kept, group strictly by world first, then by learning thread within each world (not by author/source). Only worlds that meet the cadence threshold proceed; the rest wait.

3. **Find + lock REAL sources, per world.** For each thread within a ready world:
   - If the seed already carries a full real source, use it.
   - If the seed is an open-question + named threads, find real master material (WebSearch/WebFetch; platform CLI for social sources). Lock a real, clickable source per load-bearing claim. Verify it actually says what you'll attribute.
   - A thread with no real source found = a declared GAP, never an invention.

4. **Consolidate → `PACK-<world>-<date>/ALL-ARTICLES.md`.** Full text of every locked real source for that world, each under its `# Title`. This is the NotebookLM substrate.

5. **Weave, per world.** Dispatch a capable writing agent (or write in-session) with all three non-negotiable rules verbatim, plus an explicit length ceiling. Write two files into `PACK-<world>-<date>/`:
   - `WOVEN-NARRATIVE.md` — one insightful learning through-line across that world's sources, referencing them by titles in ALL-ARTICLES.md, ending with 3–4 NotebookLM steer prompts (different POV angles) + a banned-metaphor / simplifications-to-reject list. **Length ceiling: ~400–600 words, skimmable in under 3 minutes.** Trailer for deciding whether to upload — not the learning experience itself.
   - `SOURCE-CARDS.md` — per source: verbatim load-bearing quotes + attribution + claims-allowed / claims-forbidden.

6. **Verify (do NOT skip).** Re-read `WOVEN-NARRATIVE.md` and confirm: it TEACHES; it has ZERO project-mirroring (grep for personal project names / "for you" / "this maps"); it is under the length ceiling; every reference resolves to a title in ALL-ARTICLES.md; declared gaps are honest.

7. **Assemble + render instructions, per world.** Each ready world's pack is its own 3 files. Tell the user exactly what to upload to NotebookLM (or equivalent) per pack and which steer prompt per POV.

8. **Housekeeping (every run — no accumulation, per world).** After each world's pack is verified and reported:
   - Mark consumed in-note `→ Insight Radio:` markers as done (`→ Insight Radio [packed <date>]:` or remove).
   - Delete the consumed `Inbox/seed--*.md` files (content is now in the pack).
   - Move that world's prior consumed pack to `Archive/`. One live pack per world at a time.

9. **Output.** Per world: pack location (or "not ready yet, N/3 seeds" / "held — prior pack unconsumed"), upload instruction, and which seeds were packed / routed to the parked synthesis branch / sent to GAP.

## Notes

- **Standing stack only.** Do not wire temporary or time-bounded model bursts into this command as a hard dependency.
- **Standalone + bridging.** This skill runs alone, but it's the downstream sink of idea-incubator skills, `/xenrich`, To Know cards, and vault notes. It never reaches INTO those skills; they PUSH seeds via the contract.
- **Per-project creative branch** (routing open questions on a specific film/creative project to craft interviews) is the same factory in a different feed; keep it a hand-run per-project move, distinct from the "Creative" world above (public creative-industry discourse). Don't auto-run the per-project branch here.
