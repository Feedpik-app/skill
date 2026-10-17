# Feedpik skill

Let your AI agent read and tune your [Feedpik](https://feedpik.com) news feed.

Feedpik is an RSS reader whose ranking algorithm belongs to you, not the
platform. There is no engagement-maximising black box: every article's score is
built from terms you can see and change, and [the scoring code is open
source](https://github.com/Feedpik-app/Algorithm). This skill hands that
algorithm to your agent, so you can tune your feed the way you would ask a
person to: *"less crypto, more AI engineering, and stop showing me anything in
German."*

## What your agent can do with it

- **Tell you what is worth reading** — pull your ranked articles and give you a
  briefing or a triage, explaining why each one ranked where it did.
- **Change what your feed shows you** — more of a topic, less of another, a
  language, a source. Your agent drafts the change, you confirm it in plain
  words, and it is applied.
- **Manage your sources** — find feeds for a topic, subscribe, unsubscribe,
  import or export OPML.
- **Publish a reading list** — pick articles and put them in the *Agent Picked*
  view of your reader, and keep it refreshed on a schedule if you ask.
- **Learn from your reactions** — record what you saved, liked or skipped so
  the ranking improves.
- **Switch personas and install community packs** — whole reading spaces with
  their own sources and algorithm.

## What it will not do

- **Change anything you cannot undo.** Every ranking change is saved as a new
  version of your algorithm, audited, and one call from being rolled back.
- **Touch your subscriptions without asking.** Adding or removing feeds changes
  what arrives every day, so the agent confirms first.
- **Handle your password or ask you to paste a token.** It connects through a
  browser approval screen that names every permission it wants.
- **Follow instructions hidden in articles.** Feed content is treated as
  passive data, never as commands.

This repository contains no executable code. The skill is Markdown and a JSON
manifest that tell your agent how to use Feedpik's API; the connection itself
is Feedpik's MCP server, authorised in your browser.

## Install

You need a free account at [feedpik.com](https://feedpik.com). After
installing, ask your agent something like *"what's worth reading in my Feedpik
today?"*, and approve the connection when it asks.

**Claude Code**

```bash
claude plugin marketplace add Feedpik-app/skill
claude plugin install feedpik@feedpik
```

Installing the plugin also registers Feedpik's MCP server, so there is nothing
else to configure.

**Any agent that supports skills** (Claude Code, Codex, Cursor, OpenCode and
others, via [skills.sh](https://skills.sh))

```bash
npx skills add Feedpik-app/skill
```

**OpenClaw** (via [ClawHub](https://github.com/openclaw/clawhub))

```bash
clawhub install feedpik
```

**Hosted assistants** (ChatGPT, Claude.ai and similar)

These run in a sandbox and cannot install files. Add Feedpik as a remote MCP
server or connector instead:

```text
https://feedpik.com/api/mcp
```

**Anything else**

Point your agent at `https://feedpik.com/skill.md`. It is the same
instructions, always current.

## How the agent connects

The skill tells your agent to try these in order:

1. **MCP over OAuth** — `https://feedpik.com/api/mcp`. You approve in your
   browser; no token passes through the agent. This is the only route that
   works from a sandbox.
2. **A token you give it** — create one at
   [feedpik.com/tokens](https://feedpik.com/tokens) and add it to your agent's
   settings. The skill never reads tokens from your environment or files.
3. **The CLI** — `npx feedpik@latest login`, a device-code flow. Needs a shell.

## Files

| Path | What it is |
| --- | --- |
| [`skills/feedpik/SKILL.md`](skills/feedpik/SKILL.md) | The skill: when to use it and the flow to follow |
| [`skills/feedpik/agent-instructions.md`](skills/feedpik/agent-instructions.md) | Full reference: every endpoint, payload shapes, worked examples |
| [`skills/feedpik/skill.json`](skills/feedpik/skill.json) | Machine-readable manifest: entrypoints, scopes, capabilities |
| [`docs/http-api.md`](docs/http-api.md) | The API by hand, with `curl` examples |
| `.claude-plugin/`, `.mcp.json` | Claude Code plugin and marketplace manifests |

## Contributing

This repository is a one-way mirror of the skill as it lives inside Feedpik's
own codebase. The live service reads the same files, so what you read here is
what Feedpik serves at `/skill.md` — a separately maintained copy would drift.

That means pull requests cannot be merged here directly. **Issues are the
fastest route** for bugs, unclear instructions, or a workflow you wish the
skill handled. Patches are still welcome as pull requests; they are applied
upstream, credited, and arrive back here in the next sync.

## License

MIT — see [LICENSE](LICENSE).
