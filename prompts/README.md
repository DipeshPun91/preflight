# Prompts

Standalone, copy-paste versions of the Preflight audits. Each file is
self-contained — open it, copy the prompt block, and paste it into Claude,
Cursor, ChatGPT, or any assistant with access to your codebase.

If you're using Claude Code, you probably want the slash commands in
[`.claude/commands/`](../.claude/commands/) instead — same prompts, run with
`/audit-secrets` etc. without copy-pasting.

- [01-secrets-sweep.md](01-secrets-sweep.md)
- [02-auth-teardown.md](02-auth-teardown.md)
- [03-database-interrogation.md](03-database-interrogation.md) — three sections, pick the one matching your stack: [SQL/RLS](03-database-interrogation.md#sql--postgres-rls), [Firestore](03-database-interrogation.md#firestore--firebase), [NoSQL](03-database-interrogation.md#nosql--app-level-auth-mongodb-dynamodb-etc)
- [04-input-audit.md](04-input-audit.md)
- [05-cost-bomb-check.md](05-cost-bomb-check.md)
