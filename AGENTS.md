# AGENTS.md — ~/.hermes (Hermes Agent home directory)

This is NOT a normal source repo — it is the **live runtime home** of a running Hermes Agent
instance (gateway active on Telegram, code version 0.21.3), containing user config, secrets,
state databases, and skills. The actual hermes-agent source checkout lives in `hermes-agent/`
and has its **own detailed AGENTS.md** — read it before editing any source code there.

## Directory map (what is what)

- `config.yaml` — user settings (model provider, tool loop guardrails, compression, cron, terminal). Editable, YAML.
- `profile.yaml` — display name / UI metadata for this profile.
- `SOUL.md` — the agent persona ("My AI Companion", concise Chinese/English replies).
- `.env` — **secrets only.** Never print, commit, or move credentials out of it.
- `state.db`, `state.db-wal/-shm`, `projects.db`, `kanban.db*`, `shared-state.db` — live SQLite stores. Do not modify while the gateway runs.
- `sessions/`, `cron/`, `memories/`, `logs/`, `cache/`, `images/`, `audio_cache/` — runtime data, managed by Hermes.
- `skills/` — user-facing skills (each `<name>/SKILL.md`); modify via the skill tools or `skill_manage`-style files, keep frontmatter valid.
- `plugins/`, `providers/`, `platforms/`, `hooks/` — extension points discovered at runtime.
- `bin/` — symlinked tools: `uv`, `uvx`, `browser`/`browser-use` (uv tool), `tirith`.
- `gateway.pid`, `gateway.sock`, `gateway_state.json`, `*.lock` — process bookkeeping; delete only after confirming the gateway is stopped.
- `hermes-agent/` — full source checkout of the agent (see its own AGENTS.md, ~32KB, with routing table to per-area AGENTS.md files).

## Common commands

```bash
hermes --help            # CLI entry (hermes-agent/hermes wrapper)
hermes logs --follow     # browse agent/gateway logs
hermes cron list         # scheduled jobs
hermes tools             # manage toolsets
hermes profile list      # profiles (only "default" exists here)
cd ~/.hermes/hermes-agent && source venv/bin/activate   # venv for source work
scripts/run_tests.sh     # from hermes-agent/: ALWAYS this, never bare pytest
```

## Pitfalls

- **A live gateway is running** (`gateway.sock`, PID in `gateway_state.json`). Killing it or
  mutating `state.db` / `sessions/` mid-write corrupts state; stop the gateway first for
  structural changes, and let Hermes manage lock files.
- **Never hardcode `~/.hermes` in code** — use `get_hermes_home()` / `display_hermes_home()`
  from `hermes_constants.py`; profiles make the path dynamic.
- `.env` holds secrets; `config.yaml` holds behavior settings. Don't add env vars for config.
- Don't hand-edit `state.db`, `models_dev_cache.json`, `kanban.db`, or anything `*-lock`.
- The hermes-agent source tree is large (~39k tests); its root AGENTS.md is authoritative for
  code work there — facade + `<stem>_<topic>.py` sibling layout, no change-detector tests,
  test placement mirrors source tree, upper-bounded dependency pins.
