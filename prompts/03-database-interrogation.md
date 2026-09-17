# The Database Interrogation

On Supabase, your anon key ships to every visitor's browser. Row level
security is the only thing standing between that key and your entire users
table. This prompt goes table by table and writes the exact query an
attacker would run to read someone else's rows, so you see the leak instead
of taking a policy's word for it.

## The prompt

```
Audit my database access rules table by table (Supabase RLS or equivalent).

1. List every table and whether RLS is enabled. Any table reachable with the
   public key and no RLS is CRITICAL.
2. For each policy, write the exact query user A runs to read or edit user B's
   rows, and tell me whether it works.
3. Flag policies that filter on values the client sends instead of auth.uid().
4. Check INSERT and UPDATE policies, not just SELECT: can I insert rows pointed
   at another user, or update columns I shouldn't own, like role or credits?
5. Apply the same checks to storage buckets.

Output: table | policy state | the attack query | what leaks | severity |
corrected policy as real SQL.
```
