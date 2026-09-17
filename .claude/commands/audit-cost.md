---
description: Find routes that can rack up a huge bill with no rate limits
---

1. Routes that trigger AI or LLM API calls: what stops a script from calling
   each one 100,000 times tonight? Check auth, per-user limits, per-IP limits.
2. Login, signup, and password reset: rate limits, bot protection, and whether
   response differences let someone enumerate valid emails
3. Email or SMS sending routes someone can spam through
4. Expensive queries or exports with no caps or pagination
5. Usage metering: is it enforced server-side, and does it fail closed?

Output: endpoint | what one call costs me | the abuse scenario | projected
damage from one night | severity | the exact limiter to add.
