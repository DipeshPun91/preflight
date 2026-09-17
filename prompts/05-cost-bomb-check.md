# The Cost Bomb Check

This is the one nobody warns you about. Your app has routes that cost you
money every time they run, and your AI endpoints are the worst offenders. No
rate limit means one script, one night, and a four-figure bill on your card
before you wake up. This prompt finds every route that spends your money and
asks what stops a bot from hammering it.

## The prompt

```
1. Routes that trigger AI or LLM API calls: what stops a script from calling
   each one 100,000 times tonight? Check auth, per-user limits, per-IP limits.
2. Login, signup, and password reset: rate limits, bot protection, and whether
   response differences let someone enumerate valid emails
3. Email or SMS sending routes someone can spam through
4. Expensive queries or exports with no caps or pagination
5. Usage metering: is it enforced server-side, and does it fail closed?

Output: endpoint | what one call costs me | the abuse scenario | projected
damage from one night | severity | the exact limiter to add.
```
