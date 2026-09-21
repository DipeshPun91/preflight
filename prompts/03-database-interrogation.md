# The Database Interrogation

Your database's access control lives in a different place depending on your
stack — a Postgres RLS policy, a Firestore rules file, or just whatever
check a developer remembered to write in application code — and each one
fails in a different way. Pick the section below that matches your backend
and run its prompt. If you use more than one (e.g. Postgres for the main app
plus Firestore for a feature), run each relevant section separately.

## SQL / Postgres (RLS)

On Supabase, your anon key ships to every visitor's browser. Row level
security is the only thing standing between that key and your entire users
table. This prompt goes table by table and writes the exact query an
attacker would run to read someone else's rows, so you see the leak instead
of taking a policy's word for it.

```
Audit my database access rules table by table (Supabase RLS or equivalent
Postgres row-level security).

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

## Firestore / Firebase

Firestore has no RLS — the security rules file is the only gate between the
client SDK and every document in your database. A misplaced `if true`, or a
rule that trusts a field on the incoming request instead of the caller's
verified identity, can hand over your entire dataset with no server in the
middle to catch it.

```
Audit my Firestore/Firebase security rules and how my client code queries the
database.

1. List every collection and subcollection and the exact allow read/write/
   create/update/delete conditions on it. Any collection with no matching rule
   (there is no default-deny you can assume) or an overly permissive condition
   like `if true` is CRITICAL.
2. For each collection, write the exact client SDK call user A would make to
   read or write user B's documents, and say whether the rules block it.
3. Flag rules that check a field on the incoming request or document instead
   of request.auth.uid — e.g. comparing resource.data.ownerId to a client-
   supplied value instead of to request.auth.uid.
4. Check list queries separately from get: a rule can lock down get on a
   specific document ID while still allowing an unrestricted list that returns
   every document in the collection.
5. Apply the same checks to Firebase Storage security rules, and flag any code
   reachable from a client request (e.g. an unauthenticated Cloud Function)
   that uses the Firebase Admin SDK, since that bypasses every rule above it.

Output: collection/path | rule as written | the attack call | what leaks |
severity | corrected rule.
```

## NoSQL / app-level auth (MongoDB, DynamoDB, etc.)

MongoDB, DynamoDB, and most ORMs have no database-level access control at
all — every query is only as safe as the filter a developer remembered to
write around it. This prompt checks whether that scoping actually happens on
the server, every time, for every route, instead of trusting a policy that
doesn't exist.

```
Audit how my app enforces database access at the code level (MongoDB,
Mongoose, DynamoDB, or any store with no built-in row/document-level
security).

1. List every route or resolver that reads or writes to the database. For
   each one: is the query scoped to the authenticated user's ID from the
   server-side session or token, or does it trust an ID from the request
   body, query string, or a client-supplied field?
2. For each unscoped or client-trusted query, write the exact request user A
   would send to read or edit user B's data, and say whether it works.
3. Check for mass assignment: does any create/update handler pass the whole
   request body into the database write, letting a client set fields like
   role, isAdmin, credits, or userId directly?
4. Check that every update or delete re-verifies ownership at write time, not
   just at the initial read — watch for fetch-then-check-then-write races, or
   an update filter that matches only on document ID with no owner check.
5. If a schema defines restricted fields (e.g. Mongoose `select: false`,
   immutable fields), confirm they're actually enforced on writes and not
   just excluded from reads.

Output: route/resolver | how it's scoped today | the attack request | what
leaks | severity | fix (exact query/filter change).
```
