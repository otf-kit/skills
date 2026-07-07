# ship-ready — plans reference (v1)

Turn selected findings into **self-contained, executable plans**. Read this when running `ship-ready plan`.

A plan is written for the **weakest plausible executor** — a model that never saw the audit session and may be much smaller. If the plan requires that model to reason, judge, or re-discover anything, it will get it wrong. Inline everything.

## Where plans go

Write one file per finding into `plans/`, named `NNN-short-slug.md` (e.g. `plans/001-enforce-auth-on-checkout.md`), plus a `plans/INDEX.md` with the recommended order and the dependency graph. Stamp each plan with the git commit it was written against (`git rev-parse --short HEAD`) so the executor can drift-check before touching anything.

## Required sections of every plan

### 1. Header
```
# 001 — Enforce auth on the checkout function
Finding: payments/security · Severity: blocker · Effort: S
Written against commit: a1b2c3d
Depends on: none        # or "002" if ordering matters
```

### 2. Gates preamble (run these FIRST, and again at the end)
The repo's own commands, captured in recon — stated up front so the executor never re-derives them:
```
Verification gates (this repo):
  build:      npm run build
  lint:       npm run lint
  typecheck:  npx tsc --noEmit
  test:       npx vitest run          # no `test` script; run the runner directly
```

### 3. What's wrong
The exact `file:line`(s), and **the current code excerpted inline** (not "see above"). One paragraph on the mechanism and the real-world consequence.

### 4. The fix — with a minimal before/after diff
Prose steps **plus** a minimal, literal before/after code block. **Mandatory for any security / auth / payment / multi-file-refactor finding** — prose alone lets a weak model put the early-return in the wrong place or leave the old body intact.

````
Before — functions/checkout/index.ts:
```ts
if (authHeader) {
  const r = await verifyJWT(authHeader)
  // if (!r.valid) return json({error:'unauthorized'}, 401)   // <- commented out!
}
const userId = body.userId
```
After:
```ts
const r = await verifyJWT(authHeader)          // no `if` guard
if (!r.valid) return json({ error: 'unauthorized' }, 401)
const userId = r.userId                          // from the verified token, never the body
```
````

### 5. Inline the exact API signatures
Never write "use the CRUD API" — a weak model will hallucinate method names (that is literally how the original bug was written). **Quote the real signature from the library's types**, verified against the *installed* version:
```
From @thesdk/database (installed v2.3.1), packages/sdk/src/database.ts:
  list(opts: { where?: Record<string,unknown>; orderBy?: Record<string,'asc'|'desc'>; limit?: number }): Promise<Row[]>
  update(id: string, patch: Partial<Row>): Promise<Row>
```
If the exact option shape can't be confirmed, say so and tell the executor to open the type file first — don't guess.

### 6. Enumerate multi-site changes
If the fix touches N call sites, **ship the grep that finds them all** so the executor doesn't miss any:
```
All affected sites:  grep -rn "kevinalexanderclarke@gmail.com" src
(9 occurrences — all must resolve to the single ADMIN_EMAIL constant)
```

### 7. Verification gates — positive AND falsifying
Every plan ends with machine-checkable done criteria. Two kinds:
- **Positive:** the repo's gates pass (`build` / `lint` / `typecheck` / `test` from the preamble), and a greppable self-check confirms the change is present.
- **Falsification gates (MANDATORY for any auth / payment / security fix).** State the attack that must now **fail** — because a weak executor will make the happy path work and declare victory while the bypass is still open. Prefer a runnable command over a manual multi-account scenario a small model can't stage.
  ```
  Falsification gate (must FAIL):
    curl -X POST "$FN_URL/checkout" -d '{"userId":"x"}'   # no Authorization header
    → MUST return 401. If it returns 200 with a checkout URL, the fix is INCOMPLETE.
  Self-check (must be present):
    grep -n "return.*401" functions/checkout/index.ts   → the early-return exists
  ```

### 8. Hard boundaries
- **Out of scope:** an explicit list of what this plan must NOT touch.
- **STOP conditions:** "if the code at `file:line` no longer matches the excerpt (drift), stop and report — do not improvise." "If the falsification gate can't be run, stop and report — do not mark done."

## Decompose refactors a weak model will botch

A finding like "centralize admin gating + add server-side role checks (9 sites)" is one *finding* but should become **two plans** the executor can do independently:
- `00X-a` — mechanical: extract the 9 email literals to a single `ADMIN_EMAIL` constant (gate: the grep resolves to one definition).
- `00X-b` — conceptual: add a server-side `role === 'admin'` check to each admin function (gate: a falsification test per function).

Keep each plan to one coherent, gate-able change. If you can't write a falsification or greppable gate for a step, it's too big — split it.

## INDEX.md

```
# ship-ready plans — <app name>, <date>, commit a1b2c3d
Order (by leverage; respect deps):
  1. 001-enforce-auth-on-checkout   (blocker, S, no deps)
  2. 002-verify-jwt-signature       (blocker, M, no deps)
  3. 004a-extract-admin-constant    (important, S) → then 004b
  4. 004b-server-side-role-check    (important, M, depends: 004a)
```

When the plans are written, tell the user they're in `plans/`, reviewable, and can be handed to any agent — or run via `ship-ready execute <n>` (see [`execute.md`](execute.md)).
