# Contributing

Preflight is intentionally small. Contributions that keep it that way are
the most welcome kind.

## Adding or improving a prompt

- Keep the "act as an attacker, walk every path, output a table with
  severity and an exact fix" structure — it's what makes the output
  actionable instead of a vague summary.
- A new audit prompt should live in two places: `prompts/NN-name.md`
  (standalone, with a short intro paragraph) and
  `.claude/commands/audit-name.md` (frontmatter + prompt only, no intro).
  Keep the prompt text identical between the two.
- Test the prompt against at least one real repo before opening a PR, and
  paste an example of the output in the PR description.

## Adding a new audit category

Open an issue first with the category, what class of bug it catches, and
why it's common enough to deserve a dedicated prompt rather than folding
into an existing one.

## CLI work

See [cli/ROADMAP.md](cli/ROADMAP.md) for the current plan and open
questions. Comment on an issue before starting significant work so effort
doesn't get duplicated.

## Style

- Plain markdown, no build step for the prompt library.
- Keep prompts platform-agnostic where possible — they should work pasted
  into any capable AI assistant, not just Claude.
