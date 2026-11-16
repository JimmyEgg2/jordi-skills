# AGENTS.md

This repo is a **manifest**, not a copy: `.claude-plugin/marketplace.json` lists skill sets that live in their upstream GitHub repos. Nothing here vendors skill files; every install pulls upstream.

| Plugin | Upstream repo | Shape |
|---|---|---|
| `mattpocock-skills` | `mattpocock/skills` | full plugin (own `plugin.json`) |
| `ponytail` | `DietrichGebert/ponytail` | full plugin, plus hooks that run Node on session start and every prompt |
| `security-audit` | `cloudflare/security-audit-skill` | bare skill at `skills/security-audit`, no `plugin.json` |

## Installing the skills into a local agent

When the user asks to install or update these skills for the agent you are running in:

1. **Identify the host** (Claude Code, Codex, Copilot, Cursor, Gemini CLI, OpenCode, ...). Ask if you cannot tell.
2. **Pick the path per plugin**, first match wins:
   - **Claude Code**: use this marketplace: `claude plugin marketplace add JimmyEgg2/jordi-skills`, then `claude plugin install <name>@jordi-skills` for each plugin.
   - **Host with a native plugin manager** (Codex, Copilot): install each plugin from its *upstream* repo, following that repo's README. This marketplace uses Claude Code's format and is untested on other hosts.
   - **Any other host**: the Skills CLI, user-level: `npx skills@latest add <upstream-repo> -a <agent> --global`. For Cloudflare add `--skill security-audit`. Ponytail installs as skills only this way; its hooks need a plugin host.
3. **Install each plugin once per host.** A plugin install and a Skills CLI copy of the same set load every skill twice; remove the older one.
4. **Verify**: list the host's installed skills or plugins and confirm all three sets appear at upstream's current version.

Done when every plugin in the table is installed exactly once on the host and the verification listing shows it.

## Updating the skills

**Plugin hosts** (Claude Code, Codex, Copilot) track upstream themselves: run the host's plugin update command and you are done.

**Skills CLI hosts** hold *copies*, so an update is a sync against upstream. Use the same `-a <agent>` / `--global` flags as the original install on every command:

1. **Refresh**: `npx skills@latest update` rewrites every installed skill from its upstream repo.
2. **Add new**: re-run `npx skills@latest add <upstream-repo> ...` per plugin; `update` only touches skills already installed.
3. **Prune stale**: list each upstream repo's current skill folders (`skills/**/SKILL.md`) and compare with the installed skill names. Every installed skill from one of these repos that upstream has renamed or deleted is stale; remove it with `npx skills remove <name>`. Skills renamed upstream otherwise load twice, under the old and new name.
4. **Re-apply edits**: a copy the user edited locally is overwritten by `update`. Ask before step 1 whether they edited any, and back those files up first.

Done when the installed set from these repos equals upstream's current skill list, name for name.

## Editing the marketplace

- **Add a plugin** with a `github` source pointing at the upstream repo. If the repo has no `.claude-plugin/plugin.json`, set `"strict": false` and list its skill folders in `"skills"`.
- **Pin** with `"ref"` or `"sha"` on the source only when the user asks; the default is to track upstream `main`.
- Keep the table above and `README.md` in step with `marketplace.json`.
- Run `claude plugin validate .` before committing; it must print `Validation passed`.

## Cloud sessions

`.claude/settings.json` registers this marketplace and enables all plugins for Claude Code cloud sessions on this repo. It is also the template users copy into their other repos, so keep its plugin list identical to `marketplace.json`.
