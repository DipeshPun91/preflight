# CLI Roadmap

The plan is a `preflight` CLI that runs all five audits automatically against
a target repo and prints (or writes) a single combined report, without
needing Claude Code installed.

## Rough shape

```bash
npx preflight audit ./my-app
npx preflight audit ./my-app --only secrets,auth
npx preflight audit ./my-app --output report.md
```

## Planned pieces

- **Prompt runner** — sends each `prompts/*.md` file plus relevant repo
  context to the Claude API (`claude-sonnet` by default, configurable) and
  collects the structured table output.
- **Repo context builder** — walks the target repo, respects `.gitignore`,
  and assembles a reasonably-sized context bundle per audit (secrets sweep
  needs git history; the others mostly need source files).
- **Report merger** — combines the five tables into one report, sorted by
  severity, with a rotate-these-keys section pulled to the top if the
  secrets sweep found anything.
- **Config file** — `preflight.config.json` for API key, model choice, which
  audits to run by default, and paths/globs to exclude.

## Open questions

- Should this shell out to an installed `claude` CLI, call the Anthropic API
  directly, or support both?
- How to handle very large repos that don't fit in one context window per
  audit (chunking strategy, or restrict to a `--path` the user specifies).
- Exit codes for CI use (e.g. fail the build on any CRITICAL finding).

## Contributing

If you want to pick up a piece of this, open an issue first describing which
part you're tackling so work doesn't overlap. See [CONTRIBUTING.md](../CONTRIBUTING.md).
