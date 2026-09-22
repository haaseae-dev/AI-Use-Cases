# HANDOFF.md — AI Use Case Site

Read this before touching anything. It's the "why," not just the "what."

## What this is

A single-page, static website listing real AI use cases built by Xan Schutz and Brit Vorreiter, originally created as a companion to their PMI 2026 talk ("Stop Prompting, Start Building") but intended to keep growing as a standalone thing afterward — not just a conference leave-behind.

Live at: `https://haaseae-dev.github.io/AI-Use-Cases/`
Repo: `haaseae-dev/AI-Use-Cases` (public, GitHub Pages, deployed from `main` branch root)

The site currently still says `[working name]` in the header/title — no final name has been chosen yet. Don't treat that as a bug.

## Architecture — read this before changing anything

- **One file: `index.html`.** All CSS and JS are inline in that single file. There is no build step, no framework, no package.json, nothing to compile. GitHub Pages serves it as-is.
- **Content is NOT in the file.** All use-case data lives in a Google Sheet, published to the web as CSV, fetched client-side via `fetch()` (using PapaParse, loaded from cdnjs) every time someone loads the page. The published CSV URL is hardcoded near the top of the `<script>` block as `SHEET_CSV_URL`.
- **This means: editing `index.html` changes the site's design/behavior. Editing the Google Sheet changes the site's content.** These are two completely separate update paths. Xan and Brit only ever touch the Sheet for day-to-day content; only design/feature changes require touching and re-deploying `index.html`.
- **No backend, no database, no auth.** Fully static and public.
- There's a localStorage cache (`usecases-cache-v4` key) that stores the last successful fetch, so the site still shows something if the Sheet fetch fails on a repeat visit.

## The Google Sheet — schema (source of truth for content)

The Sheet has two tabs: `Use Cases` (the real data, published to web) and `Column Guide` (a plain-English reference for Xan/Brit — **currently out of date**, only lists the original columns, missing the four added later; worth fixing but low priority).

Columns in `Use Cases`, in order:

| Column | Purpose |
|---|---|
| `Title` | Use case name |
| `Author` | `Xan` or `Brit` — matched against a hardcoded `AUTHOR_LINKS` map in the JS for LinkedIn profile links |
| `Type` | Category — drives a color-coded chip. Colors are auto-assigned from a fixed 5-color palette (`indigo, coral, mint, amber, sky`) in the order categories are first encountered, not hand-picked per category. **Wording must match exactly between rows** or it creates a duplicate category (known live data bug right now: `"Reporting"` vs `"Status reporting"`, `"Productivity"` vs `"Personal productivity"`) |
| `Problem` | Always-visible one-liner |
| `Build` | "How it was built" paragraph on the detail page |
| `Tools` | Comma-separated |
| `Level` | `Intro` / `Intermediate` / `Expert` — shown on detail page only; **the Level filter dropdown was deliberately removed** per explicit request, don't re-add without asking |
| `TimeBucket` | Must be exactly one of `Under 15 min`, `15–60 min`, `1–4 hrs`, `4–8 hrs`, `Days+` (note the en-dash). Anything else silently fails to appear in the Time filter dropdown. (`4–8 hrs` was added to `TIME_ORDER` in `index.html` specifically to give the "Notes to Executive Deck" row, previously the invalid `"4–5 hrs"`, a real home — that Sheet row still needs to be updated to the new exact string) |
| `TimeDetail` | Optional free-text elaboration |
| `Outcome`, `Lessons` | Detail-page sections, shown only if non-empty |
| `Status` | Gates visibility — see "Publish gating" below. Only 3 values do anything meaningful: `Draft` (or blank) hides the row; `Live` (exact match) publishes it normally; anything containing `"coming"` (e.g. `"Coming Soon"`) publishes it with a `" · coming soon"` tag next to the author name. Anything else is treated as not-ready and hidden, same as `Draft`. |
| `ImageURLs` | Comma-separated direct image links. **First image does double duty**: it's both the small thumbnail on the list card AND the first slide in the detail-page carousel. This was an explicit decision — don't build a separate "icon" field. |
| `ImageURL-Label` | One caption for the whole image set, not per-image |
| `HowToGuide` | Full guide, written in plain markdown, rendered inline via a small custom markdown parser in the JS (supports `#`/`##`/`###`, `**bold**`, `*italic*`, `> quote`, `-`/`1.` lists, `[link](url)`, `---`). Deliberately NOT a full markdown spec — no tables, no nested lists |
| `HowToGuide-Label` | Custom text for the inline toggle button |
| `PromptFileURL` | Link to a downloadable file (Google Drive, "Anyone with the link" + Viewer role) for guides too long/screenshot-heavy for inline rendering. Despite the name, it's general-purpose, not prompt-specific |
| `PromptFile-Label` | Button text — **does NOT rename the downloaded file**, that's controlled by the file's actual name in Drive. This distinction has caused confusion once already; keep it in mind |

### Publish gating (deliberate, don't remove)

A row only appears on the site if BOTH:
1. `Status` is exactly `Live` (case-insensitive), OR contains the word "coming" (e.g. `Coming Soon`, shown with a "· coming soon" tag) — anything else (blank, `Draft`, typos like `Done`/`Complete`, anything not on the intended 3-value list) is treated as not ready and hidden. This was deliberately tightened from an earlier, more permissive version that published on anything non-blank/non-Draft — the intent is a strict allowlist, not a denylist.
2. `Title`, `Type`, `Problem`, `Tools`, and `TimeBucket` are all non-empty — automatic backstop even if Status is set too early

If anything is hidden for being incomplete, the site shows a small message near the top of the page saying so. This was built specifically because Xan and Brit wanted a check before half-finished rows went live, without needing a second tool.

## Design decisions (so you don't relitigate settled ones)

- **Visual identity is deliberately modeled on Brit's separate "Workflow Hub" project** for brand continuity: Space Grotesk–adjacent headline font (`Bricolage Grotesque`) + `Plus Jakarta Sans` body, indigo/purple accent (`#5B4FE0`), light background, white cards, pill-shaped chips/buttons.
- **List view → detail view is real navigation**, not an accordion. Clicking a card changes the URL hash to `#/case/<id>` and swaps a `<section>`, giving each use case a shareable link and working back-button behavior, without needing multiple actual HTML files or a router library.
- **Images**: multiple images render as a click-through carousel (arrows + dots + swipe) on the detail page, NOT a "big image + horizontal scroll strip." That was explicitly tried and explicitly rejected in favor of the carousel.
- **Filters are three dropdowns** (`Type`, `Time you've got`) — actually just two now, `Level` was removed — chosen over pill/chip filters specifically because dropdowns work better as native OS pickers on mobile.
- **Search box** filters across title, problem, tools, author, and type as-you-type (the problem one-liner is included so a search term that only appears in that sentence still surfaces the card).
- A previous attempt at a dark/purple theme + this same search+detail structure was built and then explicitly reverted ("nope, go back to what we had") — the reason was never fully diagnosed (color scheme vs. structure), so if dark themes come up again, ask which specifically didn't land rather than assuming.
- Public submissions (open to anyone, not just Xan/Brit) were discussed and explicitly deferred as "phase 2" — the current site only shows what's manually added to the Sheet by the two of them.

## Content background — the domain, not just the code

**Origin and audience.** This started as the companion site for a 90-minute PMI 2026 workshop, "Stop Prompting, Start Building." The talk's own framing, useful as thematic backdrop for any copy written for the site: *prompting asks AI for an answer; building uses AI to create a repeatable way of working* — summarized in the talk as Ask → Create → Act, and separately as a three-step mental model, Choose → Build → Test. But the site itself was explicitly NOT scoped to stay PM-only — Xan decided partway through that this should read as useful to anyone building things with AI, "compiling a database of kick-ass use cases to help others," not a PM-specific resource. New entries and any rewritten copy should avoid PM-exclusive jargon accordingly, even though both current contributors are PMs by trade. (The `<title>` and meta description used to say "real AI use cases from working PMs," contradicting this decision — fixed to "real AI use cases, real builders." The visible "Started at PMI 2026 by..." origin line was left alone since that's provenance, not audience-scoping.)

**The `Type` taxonomy is emergent, not designed.** The five categories that exist (`Status reporting`, `Exec comms`, `Dashboards`, `Personal productivity`, `Process mapping`) exist because that's what the first six use cases happened to be, not because someone sat down and defined a fixed category system. New categories can and should be added freely as new use cases come in — just keep wording exactly consistent with prior use (see the data-quality bugs noted above for what happens when it isn't).

**A voice pattern worth knowing, not a rule.** Looking at the real case studies behind these entries: Xan's tend to be lighter, single-session builds — decks, video, diagrams, personal productivity tools, usually solved in one sitting to a few hours. Brit's tend to be larger, multi-day dashboard/tracker builds with heavier iteration, and her "lessons learned" often center on scope discipline and architecture (AI makes it easy to overbuild). Useful context if you're ever asked to draft a new entry's copy and want it to sound like it belongs next to the existing ones.

**There is a much larger pool of already-described use cases that never made it into the Sheet.** Real, already-written material exists — case studies, an inventory of dozens of additional use case ideas, cross-case lessons. See `CANDIDATE-USE-CASES.md`, tracked alongside this file in the repo, for what was pulled out of that material.

## Known open items / cleanup not yet done

- Site still says `[working name]` — no name has been chosen.
- `Column Guide` tab in the Sheet is stale (see schema table above).
- Two data-quality typos live in the Sheet right now (see `Type` and `TimeBucket` rows above) — need manual fixing in the Sheet, not code.
- Whether to trim the `Build` ("How it was built") summary down to a one-line teaser for rows that already have a full `HowToGuide` or `PromptFileURL` — discussed, not decided.
- No custom domain — using the default `github.io` URL. A rename of the GitHub repo was deferred until a final site name is chosen (renaming the repo changes the live URL, which would break the printed QR code).

## If you're Claude Code reading this for the first time

Don't re-derive the architecture from scratch — it's all above. The fastest way to verify current live behavior is to actually open `index.html` and check `SHEET_CSV_URL` and the column names referenced in `normalizeRows()` against what's actually described here, since the Sheet's real columns are the ground truth and this doc could drift out of sync with it over time. This file and `CANDIDATE-USE-CASES.md` are now tracked in the repo (as of the sync check that added this note) specifically so that drift is easier to catch and fix in the same commit — keep them updated when you change something they describe.
