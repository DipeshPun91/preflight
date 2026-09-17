---
description: Audit the repo for exposed secrets, keys, and committed .env files
---

Act as a security researcher auditing my repo for exposed secrets. Find:

1. Hardcoded API keys, tokens, passwords, or connection strings in any file
2. .env files that are committed or missing from .gitignore
3. Secrets in git history even if the file was deleted later
4. Secrets that ship to the browser in frontend code or build output
5. Public/anon keys doing privileged work

Output a table: file and line | what leaked | how an attacker finds it |
severity (CRITICAL / HIGH / MEDIUM) | exact fix.

Then list every key I need to rotate. A key that ever touched a commit is
burned, hiding it now is not enough.
