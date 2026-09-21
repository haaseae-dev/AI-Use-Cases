# CANDIDATE-USE-CASES.md

Real use cases Xan and Brit have already described in planning materials, but that haven't been turned into Sheet rows yet. This is raw material for future entries — not polished copy, and not everything needs to make the cut. Pull from here rather than starting from a blank page.

Each one below has enough detail already written to turn into a Sheet row (`Type`, a `Build` paragraph, `Tools`, `Outcome`, `Lessons`) with light editing.

---

## From Xan's own work

**Video from Tickets/Metrics** — Take a ticket dump (with comments) as CSV, feed it to ChatGPT for KPI/sentiment analysis, pass the output to Gemini for a clean presentation, then use Google Vids to turn it into a narrated video (editable voiceover, transitions, slide content). *(Note: a version of this — "Ticket-to-Video Workflow" — is already in the Sheet; this is the same idea, kept here in case the original framing is useful for a "what to try next" style entry.)*

**PMO Intake & Scoring Agent** — Built a Rovo agent for PMO intake and steer-co approval scoring: create the agent, paste a written prompt into its Instructions field, attach 6 supporting skills, turn on org knowledge plus two pinned PMO pages, test against a real ticket. Noted as "worked a bit, but still complex" — a good candidate for the "failure/pitfall" framing rather than a clean win.

**Complex Process Breakdown (Confluence → draw.io)** — Already in the Sheet as "Messy Docs to a Clean Process Diagram," with its full how-to guide attached.

**Self Performance Review Prompt** — A long, structured prompt (works in any LLM, best in the one with the most history on you) that asks the AI to review 3 and 6 months of chat history as evidence of professional performance: executive assessment, strengths/growth areas, a 3-vs-6-month trajectory comparison, a leadership capability rating across a dozen dimensions, and "hard feedback you may not want to hear." Strong candidate for `Personal productivity`, `Expert` level, and this one is a natural fit for the `PromptFileURL` download mechanism given its length.

---

## From Brit's documented case studies

Each of these already has a full step-by-step writeup, tools list, timeframe, and lessons-learned section in the source planning doc — they just need trimming into the Sheet's shorter fields, with the full version optionally becoming a `HowToGuide` or downloadable file.

- **Project Resource Constraint Dashboard** — already in the Sheet as "Capacity Dashboard, CFO-Grade."
- **Labor Synergies / Workforce Transition Dashboard** — a labor-savings and transition-risk tracker that pivoted from an overbuilt 10-tab Excel sheet to an interactive HTML dashboard with a "Hide Names" privacy toggle. `Dashboards`, `Expert`, `Days+`.
- **Non-Labor Synergies Tracker** — a kanban-style savings tracker (Identified/Planned/Achieved) built in a single session, with Excel import/export and a portability fix after a localStorage data-loss scare. `Dashboards`, `Expert`, likely `1–4 hrs` for v1.
- **Integration Roadmap Gantt** — a native-Excel executive roadmap (deliberately NOT an app) with monthly and quarterly views, built to paste directly into PowerPoint. Good example for "sometimes the best AI-assisted solution is still Excel."
- **Executive Project Status Slide Generator** — already in the Sheet as "Status Notes → Editable Slide."
- **Reusable AI Job-Search Skill** — converted an existing job-search SOP into a Claude skill: automated LinkedIn scraping, scoring against hard rules, Excel tracker generation, and a resume-tailoring flow using real Word tracked changes instead of a chat-based review. `Process mapping` or a new `Automation` type, `Expert`.
- **Goals & Life Map** — a personal planning app (goals, habits, daily agenda, milestones) with cross-device sync via Supabase, built and hosted on Netlify. `Personal productivity`, `Expert`, `Days+` (weeks, really — may need a bucket wider than the current max).
- **PM AI Workflow Hub** — Brit's own separate use-case directory site (the one whose design system this site borrowed fonts/colors from). Worth a self-referential entry: "we built a whole other site this way too."

---

## The broader inventory (titles only — not yet fleshed out)

These were named in planning but don't have full write-ups yet. Listed here so they're not lost, not because they're ready to add as-is:

- Create a Presentation from Messy Documentation (general version, beyond the one exec-deck example already in the Sheet)
- Turn Information into a Workflow Diagram (general version)
- Build an AI Chief of Staff (in development / conceptual)
- Automatically Document What You Build (Brit's own `BUILD_LOG.md` habit — meta, but genuinely a use case)
- Build a Portfolio of Your AI Use Cases (i.e., this site, recursively)
- Review Documents Against a Rubric + Build a Decision Dashboard (used for reviewing conference speaker submissions)
- Turn Research into a Personalized Learning System (NotebookLM)
- Turn Research into an Instructional Deck (NotebookLM)
- Turn Dense Information into a Podcast (NotebookLM audio overviews)
- Use Voice Mode as a Thinking Partner
- Create a Purpose-Built AI Agent (Copilot)
- Build a Website Without Being a Developer (i.e., this exact site)
- Rapidly Prototype an Executive Project Dashboard (Google AI Studio)
- Mock Up an Idea Before You Build It
- Improve an Existing Slide Instead of Starting Over (Gemini/Slides "beautify")
- Use Multiple AI Tools as a Workflow (same task run through several tools, best parts combined)
- Turn Your Own Use Cases into a Dashboard (recursive again)

## Failure / pitfall cases — worth their own category or framing

These are explicitly documented as things that went wrong or should give pause — good candidates for an honest, non-hype-y counterpoint to the win stories:

- **When AI Shouldn't Make the Decision**: an attempt to have AI infer billable project time from email/calendar/transcripts for CapEx/OpEx purposes — a case for not trusting AI with auditable, high-stakes decisions.
- **Check the Work**: an AI-generated earned-value calculation turned out wrong; caught by having a second AI check the first one's math.
- **When Iteration Makes the Output Worse**: a diagram that was genuinely good after 10 minutes got worse after an hour of over-iterating — the lesson being to decide "good enough" up front and know when to stop.

---

*This file is a snapshot from the chat that built the original site. It's now tracked in the repo alongside `HANDOFF.md`, so future edits can happen by hand here directly rather than only in chat — if Xan or Brit want it refreshed with newer material, edit this file and commit.*
