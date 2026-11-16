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

## Editing the marketplace

- **Add a plugin** with a `github` source pointing at the upstream repo. If the repo has no `.claude-plugin/plugin.json`, set `"strict": false` and list its skill folders in `"skills"`.
- **Pin** with `"ref"` or `"sha"` on the source only when the user asks; the default is to track upstream `main`.
- Keep the table above and `README.md` in step with `marketplace.json`.
- Run `claude plugin validate .` before committing; it must print `Validation passed`.

## Cloud sessions

`.claude/settings.json` registers this marketplace and enables all plugins for Claude Code cloud sessions on this repo. It is also the template users copy into their other repos, so keep its plugin list identical to `marketplace.json`.
