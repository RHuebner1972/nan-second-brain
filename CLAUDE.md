# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What This Repo Is

This is a **Claude Code plugin** (`the-brain`) — a personal second-brain system distributed via the Claude Code marketplace. It contains no application code to build or test; everything is markdown skill definitions and plugin metadata.

Install and use it:
```
/plugin marketplace add RHuebner1972/nan-second-brain
/plugin install the-brain@the-brain
```

Update it:
```
/plugin update the-brain@the-brain
```

## Repo Layout

```
.claude-plugin/
  plugin.json        # Plugin manifest (name, version, userConfig)
  marketplace.json   # Marketplace listing
skills/
  <skill-name>/
    SKILL.md         # Agent instructions for that skill
  newbrain/
    assets/
      CLAUDE.md      # Brain schema template — installed into every new Brain by /newbrain
      templates.md   # Page templates installed into every new Brain
```

There is no build step, no test suite, and no package manager. All development is editing markdown files.

## Plugin Architecture

The plugin exposes **15 skills**, each defined by a single `SKILL.md` file. When a user invokes `/the-brain:<skill>`, Claude Code loads that skill's `SKILL.md` and Claude follows it as agent instructions.

Skills are **agent orchestration scripts**: they tell Claude which tools to call, in what order, and under what conditions. They reference the Brain's own `CLAUDE.md` schema (installed into the user's Brain folder by `/newbrain`) as the source of truth for data conventions.

The `brain_path` plugin setting (in `~/.claude/settings.json` under `pluginConfigs`) lets four "anywhere-skills" (`myday`, `standup`, `prep`, `log`) locate the Brain from any working directory.

## The Brain System (What Skills Operate On)

Skills create and maintain a **Brain** — a local folder with three sub-brains:

- `wiki/` — The Scholar: concepts, entities, source summaries, long-term knowledge
- `pm/` — The Project Manager: active projects, decisions, meeting pages, institutional memory
- `planner/` — The Executive Assistant: daily plans (`planner/YYYY/MM/YYYY-MM-DD.md`), tasks, reflections
- `raw/` — Immutable source archive (verbatim static files + `.ref.md` connector-reference stubs for Outlook/Teams content — never verbatim email/transcript bodies)
- `CLAUDE.md` — Brain's own schema (installed from `skills/newbrain/assets/CLAUDE.md`)
- `log.md` — Brain operational changelog

## Key Conventions When Editing Skills

**The Brain's CLAUDE.md (in `skills/newbrain/assets/`) is the authoritative schema** for all data conventions (linking, page types, frontmatter, raw-first rules, connector tiers). Skills must stay consistent with it.

**Three-tier data governance** governs what goes in `raw/`:
- Tier 1 (static sources → saved verbatim): articles, PDFs, handwritten notes
- Tier 2 (Outlook/Teams → `.ref.md` stub only, never verbatim): emails, Teams chats, transcripts
- Tier 3 (Jira, Confluence, live systems → frontmatter/deep links only, nothing in `raw/`)

**All internal Brain links use `[[wikilinks]]`** — never markdown `[text](path)`.

**`allowed-tools` in skill frontmatter** restricts what tools that skill can use. Keep this minimal and intentional.

**Skills never write to the plugin directory** (`${CLAUDE_PLUGIN_ROOT}`). All Brain writes go to the user's Brain folder.

**Draft-first skills** (`compose`, `ticket`) always show a draft to the user and wait for approval before any external action.

## Versioning

Bump `version` in `.claude-plugin/plugin.json` to publish an update. Users update via `/plugin update the-brain@the-brain`.
