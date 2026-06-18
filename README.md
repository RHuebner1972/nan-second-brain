# the-brain

A personal knowledge system operated entirely by Claude. Three specialized sub-brains work together — **wiki/** (The Scholar: concepts, entities, source summaries, long-term semantic memory), **pm/** (The Project Manager: active projects, decisions, meeting notes, institutional memory), and **planner/** (The Executive Assistant: daily plans, activity logs, tasks, weekly reflections) — all connector-aware, all self-documented in a CLAUDE.md schema that any LLM session can read cold and immediately understand. You curate sources and ask questions; Claude writes and maintains everything.

---

## Install

```
/plugin marketplace add nelnet-nbs/ailab-the-brain
/plugin install the-brain@the-brain
```

Skills are shown throughout this README by their short names (e.g. `/newbrain`, `/myday`). The fully-qualified form `/the-brain:<skill>` always works too — it's only *required* if another installed plugin defines a skill with the same name.

> **Note:** `brain_path` only applies when the Brain is configured and used in **Claude Code**. Claude Cowork users don't need it and can't set it — Cowork sessions are already rooted in the Brain folder, so this setting can be ignored there.

---

## Create your Brain

```bash
cd ~/my-brain   # or any empty folder
claude
```

Then run:

```
/newbrain
# or point it at a path:
/newbrain ~/path/to/brain
```

`/newbrain` scaffolds the full directory structure (`wiki/`, `pm/`, `planner/`, `raw/`), installs the CLAUDE.md schema, page templates, and starter indexes, then walks you through timezone and connector configuration. It also sets the plugin's `brain_path` setting to your new Brain automatically, so `/myday`, `/log`, `/standup`, and `/prep` work from any directory — not just when you've `cd`'d into the Brain. (You can change it later with `/plugin configure the-brain@the-brain`.)

---

## Daily habit

| Skill | What it does |
|---|---|
| `/myday` | Morning briefing — schedule, priorities, and daily plan from calendar + Brain context |
| `/ingest` | Process a raw source (article, PDF, notes) into the Brain |
| `/met` | Acquire a meeting transcript from Outlook/Teams, save a stub to `raw/`, and ingest |
| `/ask` | Query the Brain with cited, claim-level sources |
| `/project` | View or create a project page with live connector data |
| `/standup` | Generate a standup summary (yesterday, today, blockers) formatted for Slack or reading aloud |
| `/prep` | Assemble a meeting briefing from project state, recent history, and live connectors |
| `/log` | Append an activity log entry to today's daily file |
| `/reflect` | Generate a daily or weekly reflection; prune completed tasks |
| `/lint` | Health-check the Brain — orphan pages, broken links, stale content, raw/ governance violations |

The full catalog (`/compose`, `/ticket`, `/sync`, `/plot`, and more) is documented in the Brain's `CLAUDE.md` after you run `/newbrain`.

---

## Data governance

Outlook emails, Teams chats, and Teams meeting transcripts are **never archived verbatim** in `raw/`. Instead, `/met` and related skills write connector-reference stubs — metadata plus retrieval instructions — so content is re-pulled live via connectors on demand. Derived notes, decisions, and meeting summaries live in the Brain as normal pages. `/lint` audits `raw/` and flags verbatim Outlook/Teams content as violations.

Why: source-system retention policies (90-day email, 180-day transcripts) remain authoritative, and duplicating that content in the Brain creates stale, out-of-policy copies.

Static sources — articles, PDFs, SharePoint exports, handwritten notes — are still archived verbatim in `raw/` per the raw-first rule.

---

## Updating

```
/plugin update the-brain@the-brain
```

Updates land when the plugin version is bumped in the marketplace manifest.

---

## Repo layout

```
.claude-plugin/     # plugin manifest + marketplace listing
skills/             # 15 skills, each in its own subdirectory
  newbrain/
    assets/         # CLAUDE.md schema + page templates installed into every new Brain
  ingest/
  met/
  ask/
  ...
```
