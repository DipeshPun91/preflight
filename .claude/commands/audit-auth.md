---
description: Attack the app's authentication and session handling
---

Attack my authentication like you want in. Walk every auth path in this repo:

1. List every route and API endpoint and whether it verifies a valid session.
   Flag any that skip it.
2. Session handling: where tokens live, whether they expire, whether logout and
   password change kill them
3. Password rules: minimum length, breached-password check, anything my auth
   provider offers that I left off
4. Password reset and email verification: can I reset someone else's password,
   or use the app with an unverified email?

Output: flow | weakness | exact exploit steps | severity | fix.

Any endpoint that trusts a user ID from the request body instead of the session
is CRITICAL.
