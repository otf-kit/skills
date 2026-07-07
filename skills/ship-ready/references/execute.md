# ship-ready — execute reference (v2)

Hand a plan to a cheaper executor, verify it, and review it like a tech lead. Read this when running `ship-ready execute <plan>` or `ship-ready reconcile`.

The point of `execute` is to use an **expensive** model for judgment (the audit + the plan) and a **cheap** model for the mechanical work — *without* trusting the cheap model's own "done". The cheap model will make the happy path work and self-verify green while a security bypass is still open. So execution is: isolate → run → **prove the attack now fails** → review the diff → verdict. Merging always stays with the user.

## Preconditions

- The plan exists in `plans/` and has a **gates preamble** + **falsification gates** (from [`plans.md`](plans.md)). If a security/auth/payment plan has no falsification gate, **do not execute it** — send it back to planning. A positive-only gate ("build passes") is theater for these.
- The repo is a git repo with a clean working tree (or the user has committed/stashed). If it isn't a git repo, say so — execution needs isolation and you won't create it on the user's live tree.

## The protocol

### 1. Isolate
Create a fresh worktree + branch off the current HEAD — never touch the user's working tree:
```
git worktree add ../ship-ready-001 -b ship-ready/001-enforce-auth
```
All execution happens in `../ship-ready-001`. If the plan's "Written against commit" ≠ current HEAD, run the plan's **drift check** first; if the cited code no longer matches the excerpt, STOP and report — do not improvise.

### 2. Dispatch the cheap executor
Hand the executor **only** the plan file + the worktree path. It has never seen the audit session, so the plan must stand alone (it does, if `plans.md` was followed). Instruct it explicitly:
- Make exactly the change in the plan. Respect the out-of-scope list. Touch nothing else.
- Do **not** decide the plan is done from the happy path — the plan's falsification gate decides.
- If reality doesn't match the plan (drift, a missing file, an API that doesn't exist), STOP and report — do not invent a workaround.

Use a smaller/cheaper model here if your agent supports model selection; this is mechanical work.

### 3. Verify — run the gates yourself, don't trust the executor's word
Back in the worktree, **re-run every gate in the plan** yourself:
- **Positive gates:** `build` / `lint` / `typecheck` / `test` from the preamble must pass.
- **Falsification gates (the load-bearing ones):** run the attack. It **must fail** now.
  - `curl` the endpoint with no token → must `401`, not `200`.
  - Where the gate is a multi-account scenario a script can't easily stage, either script it (seed two users via the SDK/API) or, if truly manual, **mark the finding "needs human verification"** in the verdict — do **not** pass it on the executor's say-so.
- **Self-check greps:** confirm the change is present where the plan said (`grep -n "return.*401" ...`).

A plan is **not** done until its falsification gate has actually been observed to fail the attack. Positive gates green + falsification gate un-run = **incomplete**.

### 4. Review like a tech lead
Read the diff against the plan's intent, not just its letter:
- **Scope:** did it touch only what the plan allowed? Flag any drive-by edits.
- **Completeness:** for a multi-site fix, run the plan's enumeration grep — are *all* sites changed, or did it fix one and miss eight?
- **The subtle-security trap:** did it "fix" #1 by adding a token on the client while leaving the server's optional-auth guard in place? Read the *server* control flow, not the client call.
- **No new holes:** did the change introduce a different gap (e.g. now throwing on a valid request)?

### 5. Verdict
Report one of:
- **PASS** — gates pass, falsification gate observed failing the attack, scope clean. Diff is in branch `ship-ready/001-...`; the user reviews + merges.
- **PASS (needs human verification)** — code looks right but a falsification gate could only be checked manually. Say exactly what the human must run.
- **FAIL** — what's still open (quote the falsification gate that still passes the attack, or the missed sites). Leave the branch for inspection; do not merge, do not retry blindly more than once.

Never merge. Print the branch name and the exact `git diff --stat`, and hand it back.

### 6. Clean up
If the worktree is unchanged or the run was abandoned, remove it: `git worktree remove ../ship-ready-001`. Leave it in place on a PASS/FAIL the user will inspect.

## `ship-ready reconcile`

Refresh the backlog so it reflects reality, not the day the plans were written:
- **Verify landed work:** for each plan, re-run its falsification + positive gates against the *current* HEAD. If the attack now fails and gates pass, mark the plan **done** and move it to `plans/done/`.
- **Drift-check the rest:** for each open plan, run its drift check (does the cited `file:line` still match the excerpt?). If the code moved, mark the plan **stale** — it needs a fresh audit pass on that spot, not a blind execute.
- **Unblock:** if a plan's dependency is now done, mark it ready.
- **Retire:** if a finding is no longer reachable (feature removed, file gone), retire it with a one-line reason so it doesn't resurface next audit.

Report the refreshed state: done / ready / blocked / stale / retired, with counts.

## Guardrails (repeat of the hard rules that matter most here)

- Execution happens **only** in an isolated worktree. Never the user's working tree. Never a merge.
- A weak executor's "done" is a hypothesis; the **falsification gate** is the proof. No falsification gate on a security fix → don't execute it.
- No telemetry. No network calls except what the plan's gates explicitly run against the app under test.
- One retry on FAIL, then hand it back to a human — don't let a small model thrash on a spec it can't satisfy.
