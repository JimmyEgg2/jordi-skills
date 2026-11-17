# jordi-skills

A Claude Code plugin marketplace that bundles skill sets straight from their upstream GitHub repos, so every install/update gets the latest version.

| Plugin | Upstream |
|---|---|
| `mattpocock-skills` | [mattpocock/skills](https://github.com/mattpocock/skills) |
| `ponytail` | [dietrichgebert/ponytail](https://github.com/dietrichgebert/ponytail) |
| `security-audit` | [cloudflare/security-audit-skill](https://github.com/cloudflare/security-audit-skill) |
| `impeccable` | [pbakaus/impeccable](https://github.com/pbakaus/impeccable) |
| `diagram-design` | [cathrynlavery/diagram-design](https://github.com/cathrynlavery/diagram-design) |

**Not a plugin, install separately:** [Graphify](https://github.com/Graphify-Labs/graphify) is a Python CLI that registers its own skill:

```
uv tool install graphifyy
graphify install
```

**Worth a look, not included:** [Anatomy](https://github.com/wheresryan22/anatomy) explains an idea by building it as an interactive isometric SVG/WebGL figure.

## Local Claude Code CLI

```
/plugin marketplace add JimmyEgg2/jordi-skills
/plugin install mattpocock-skills@jordi-skills
/plugin install ponytail@jordi-skills
/plugin install security-audit@jordi-skills
/plugin install impeccable@jordi-skills
/plugin install diagram-design@jordi-skills
```

## Claude Code cloud sessions

Copy [`.claude/settings.json`](.claude/settings.json) into any repo you open in a cloud session and commit it. Each new session installs the marketplace and enables the plugins automatically.

## Adding a plugin

Add an entry to [`.claude-plugin/marketplace.json`](.claude-plugin/marketplace.json):

```json
{ "name": "my-plugin", "source": { "source": "github", "repo": "owner/repo" } }
```

If the repo has no `.claude-plugin/plugin.json` (bare skills only), add `"strict": false` and `"skills": ["./path/to/skill"]`.

To pin a version, add `"ref": "<tag-or-branch>"` or `"sha": "<commit>"` to the `source`.

## Other local agents (Codex, Copilot, Cursor, Gemini, OpenCode, ...)

Open this repo in your agent and ask it to "install these skills". [`AGENTS.md`](AGENTS.md) tells it which install path to use for its host.
