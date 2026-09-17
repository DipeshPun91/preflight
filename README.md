# 🛫 Preflight

**Security audit prompts for apps built fast with AI.**

Preflight is a small, opinionated set of audit prompts that catch the mistakes
vibe-coded and AI-assisted apps ship with most often: leaked keys, missing
auth checks, wide-open database policies, unsanitized input, and routes that
can rack up a huge bill overnight.

Run them against your repo with Claude Code, Cursor, or any AI coding
assistant before you push to production — think of it as a pre-flight
checklist, not a replacement for a real pentest.

## Why this exists

Most "is my app secure?" checklists are generic. These five are written for
a specific failure mode: code that works, was shipped fast, and was never
looked at by anyone who thinks like an attacker. Each prompt tells the model
to act like an adversary, walk every relevant path in the codebase, and
output a table of findings with severity and an exact fix — not a vague
"looks okay" summary.

## The five audits

| # | Audit | Catches |
|---|-------|---------|
| 1 | [Secrets sweep](prompts/01-secrets-sweep.md) | Hardcoded keys, committed `.env` files, secrets buried in git history |
| 2 | [Auth teardown](prompts/02-auth-teardown.md) | Routes that skip session checks, endpoints that trust a client-supplied user ID |
| 3 | [Database interrogation](prompts/03-database-interrogation.md) | Missing/weak RLS policies, one user reading or editing another's rows |
| 4 | [Input audit](prompts/04-input-audit.md) | Injection, unsafe `eval`/shell calls, unsanitized uploads, unescaped HTML |
| 5 | [Cost bomb check](prompts/05-cost-bomb-check.md) | Unrated AI/LLM routes, missing rate limits, spammable email/SMS endpoints |

Run all five before you ship anything with auth, a database, user input, or
a paid API in the critical path.

## Usage

### Option A — Claude Code slash commands (recommended)

Copy the `.claude/commands` folder into the root of the project you want to
audit:

```bash
curl -L https://github.com/<your-username>/preflight/archive/refs/heads/main.tar.gz \
  | tar -xz --strip-components=2 -C ./.claude/commands preflight-main/.claude/commands
```

(or just clone this repo and copy the folder by hand). Then, inside Claude
Code, run:

```
/audit-secrets
/audit-auth
/audit-database
/audit-input
/audit-cost
```

Each command runs the full prompt against your current project.

### Option B — copy-paste into any AI tool

Each file in [`prompts/`](prompts/) is a standalone prompt. Open one, copy
the whole thing, and paste it into Claude, Cursor, ChatGPT, or whatever
assistant has access to your codebase.

### Option C — CLI (planned)

A `preflight` CLI that runs all five audits automatically and prints a
combined report is on the roadmap — see [`cli/ROADMAP.md`](cli/ROADMAP.md).
Contributions welcome.

## A note on how to read the output

These prompts ask for severity ratings and exact fixes, but the model can
still miss things or misjudge severity — treat the output as a strong first
pass, not a certificate of security. Anything rated CRITICAL is worth a
second look by a human before you dismiss or fix it.

## Contributing

New audit categories, better prompts, and CLI code are all welcome — see
[CONTRIBUTING.md](CONTRIBUTING.md).

## License

MIT — see [LICENSE](LICENSE).
