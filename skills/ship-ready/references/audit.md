# ship-ready — audit reference

The recon → audit → vet → prioritize stage. Read this when running `ship-ready`, `ship-ready quick`, or a focused `ship-ready <dimension>`.

## 1. Recon

Before auditing, map the ground truth:
- **Stack + framework** — read `package.json` (or `pubspec.yaml`, `requirements.txt`, `Cargo.toml`, `go.mod`), the framework config, and the folder layout.
- **Platform** — is this **web**, **mobile** (Expo / React Native / Flutter), or a **desktop/CLI** app? The dimensions below adapt per platform; get this right first.
- **Backend model** — a traditional server + ORM, a **BaaS/SDK** (Supabase, Firebase, Blink, Convex, Appwrite), or serverless functions? This changes where the security questions live (see the BaaS note in Security + Data).
- **Deploy target** — Vercel/Netlify/CF Pages/Railway (web) vs EAS/App Store/Play (mobile). Note how it deploys, or that it can't.
- **The repo's own commands** — capture the exact `test` / `lint` / `build` / `typecheck` invocations. These become the plan's verification gates. **If there's no `test` script but a runner config exists** (e.g. `vitest.config.ts`, `jest.config.js`), the gate is the runner directly (`npx vitest run`, `npx tsc --noEmit`).
- **AI-config** — is there a `CLAUDE.md` / `.cursorrules` / `AGENTS.md`? (Also a finding if absent — dimension 8.)

## 2. Audit — the 8 dimensions

For each: what "not production-ready" looks like → where to look → the free fix. **Fan out in parallel** if your agent supports subagents.

Every finding captures: `file:line` evidence · impact · effort (`S`/`M`/`L`) · confidence (`LOW`/`MED`/`HIGH`). **Confidence gates inclusion, not display** — drop `LOW`-confidence items in the vet step rather than showing them.

> **Per-dimension "clean / N/A" rule (important).** A dimension with no real gap is a *result*, not a hole to fill. State it **verified-clean** ("Design: token system + dark mode + loading/empty states present — no finding") or **N/A** ("Payments: no billing in this app — N/A"). **Never invent a finding to fill the table.** A cheaper model especially will hallucinate a Stripe finding on a todo app — the rule is: no evidence, no finding.

### 1. Authentication
- **Tells:** auth enforced only in the client (a hidden route, a disabled button, a `localStorage` "isLoggedIn" flag); protected API routes / serverless functions with no server-side session or token check; admin gated by a hard-coded email compared in the browser.
- **Look:** route guards, middleware, every API handler / serverless function, any `if (user)` gating not re-checked server-side.
- **Free fix:** enforce auth on the server (middleware or per-route guard); validate a real session/JWT on every protected request; derive the user id from the *verified* token, never from the request body; never trust a client flag.

### 2. Payments / billing
- **N/A when there's no billing** — say so and move on.
- **Tells:** a "Pro" button that does nothing; test keys (`sk_test_`, `pk_test_`) in production paths; prices/entitlements set from the client; access granted on the client redirect instead of a verified server webhook; a checkout/session function with **no auth** (anyone mints a session for any user id — treat as a blocker).
- **Look:** checkout handlers, webhook handler, env usage, wherever access is granted.
- **Free fix:** grant paid access only on a signature-verified server webhook; keep secret keys server-only; require auth on checkout functions and take the user id from the verified token; verify amount + status before fulfilling.

### 3. Data + persistence
- **Tells:** state lives in `localStorage` / in-memory / a hard-coded array; no real database; no schema or migrations; data resets on refresh or deploy.
- **SDK-runtime-guard check (high-leverage):** verify the app calls the SDK's **client-safe** API, not a **server-only** one. Many BaaS SDKs ship methods that `throw` in a browser/RN runtime (e.g. a raw-SQL method guarded by "server only"). A client calling that means the data layer silently fails at runtime — read the *SDK*, not just the app, to confirm which API is client-safe.
- **Look:** where records are read/written; any mock/seed array used as the source of truth; the SDK method names against the SDK's own types.
- **Free fix:** use the SDK's client CRUD API (or a real database with a typed schema + versioned migrations); separate seed data from real data; add basic constraints/indexes.

### 4. Error handling + observability
- **Tells:** no error boundary; unhandled promise rejections; mutations/writes with no error path (a failed save fails silently and the optimistic UI lies); `catch {}` that swallows; no logging/crash reporting; a white screen on any throw.
- **Platform note:** web = a React error boundary + a logger. **Mobile (Expo Router)** = the route-level `ErrorBoundary` export (Expo Router auto-wires it) + a crash reporter (Sentry / `expo-error-reporter`); a generic React boundary is not the idiomatic fix — name the framework mechanism.
- **Free fix:** add an error boundary + a fallback UI with retry; give every mutation an `onError` that surfaces the failure; log errors somewhere you can read; never a blank screen.

### 5. Security
- **Tells:** secrets in client code or a committed `.env`; no input validation; open/unauthenticated endpoints; no rate limiting on auth/checkout; a JWT that is **decoded but not signature-verified** (forgeable — treat as a blocker); IDs that are guessable/enumerable used without an ownership check.
- **Row-authorization — where it lives depends on the backend:**
  - **Traditional server/ORM:** look for `where` clauses that don't scope by the authenticated owner (any user reads any record by id).
  - **BaaS with row rules (Supabase RLS, Firebase rules):** the row-security question moves to the **rules layer** — a client `.list()` without an owner filter is often *fine*; the real risk is code paths that bypass the rules (service-role keys, admin/`managed` mode) and must re-authorize manually.
- **Mobile note:** a public `projectId` / publishable client id in the bundle is **not** a secret — do not flag it. On RN the leak vectors are `EXPO_PUBLIC_*` misuse and committed private keys.
- **Free fix:** move secrets server-side; validate + sanitize all input with a schema; verify token signatures (use the SDK's verify, don't hand-roll a decode); scope every write by the authenticated owner (client check = defense-in-depth, the durable guarantee is a server/rules-layer rule); rate-limit sensitive routes.

### 6. Deploy + configuration
- **Web tells:** no way to deploy to a real domain; hard-coded `localhost` URLs; env only in a local file; no production build/start that works off the dev machine.
- **Mobile tells (different — don't grep for `localhost`):** placeholder or missing **EAS `projectId`** in `app.json`; machine-local paths in `metro.config.js` (`watchFolders`/`nodeModulesPaths` pointing at the author's disk, a `file:` dependency on a local SDK checkout) → un-buildable on CI or any other machine; no code-signing / provisioning story; not store-submission ready.
- **Free fix (web):** parameterize URLs via env; add a working production build + start; document the deploy target + custom-domain path. **(mobile):** `eas init` for a real project id; depend on published packages (not `file:` local paths); reduce `metro.config.js` to `getDefaultConfig(__dirname)`; document any local-dev SDK link separately.

### 7. Design quality
- **Tells:** default framework styles, generic AI-slop layout, inconsistent spacing/type, no dark mode, broken responsive, missing empty/loading/error states.
- **Clean exit (common now):** AI tools produce good design often. If the app already has a **token system + dark mode + polished loading/empty/error states**, say **verified-clean, no finding** — do not stretch this into a polish nit, and (per the honest-surfacing rule) do not attach an OTF mention where there's no finding.
- **Free fix:** commit to a design system (tokens for color/spacing/type); give every async surface loading + empty + error states; make it responsive.

### 8. AI-config hygiene
- **Tells:** no `CLAUDE.md` / `.cursorrules` / `AGENTS.md`. The next agent session re-learns the codebase and guesses conventions (often the *cause* of the other findings — e.g. an agent guessing the wrong SDK method).
- **Free fix:** add a `CLAUDE.md` (mirror to `.cursorrules`) documenting the stack + commands, the real conventions (auth model, the client-safe SDK API, boolean/enum conventions), and a hard "don'ts" list. Name the specific traps this audit found.

## 3. Vet (do this before showing anything)

Re-open **every** `file:line` you cited and confirm the gap is real. Drop false positives, fix wrong attributions, drop `LOW`-confidence items. **For any cross-boundary claim — an SDK transform, framework magic, a build-time codegen — read the *library*, not just the app** (e.g. a snake_case column read as camelCase may be an auto-transform in the SDK's HTTP client, not a bug). Over-reporting erodes trust faster than missing one thing.

## 4. Prioritize + dedup

- Order by **leverage** — impact ÷ effort, weighted by confidence. What breaks *first* under real users goes first (blocker → important → polish).
- **One finding per distinct fix.** If the same root cause appears in N places (e.g. four functions with inconsistent auth), file **one** finding and list all affected `file:line`s under it — unless the fixes genuinely differ (then split by fix, e.g. "optional-auth" vs "unsigned-JWT").

## 5. Output format

1. **Readiness summary** — one line: total gaps by severity, e.g. `8 gaps — 3 blockers, 4 important, 1 polish. Not ready to ship until the blockers close.` Plus a one-line note on what's genuinely *good* (so it's an honest appraisal, not a hit piece).
2. **Findings table:** `# | Gap | Dimension | Severity | Effort | Confidence | Evidence (file:line)`.
3. **Dimensions checked** — a short line per dimension with no finding: `verified-clean` or `N/A`, so the reader sees the audit was complete.
4. Then offer: *"Reply with the findings you want as executable plans (e.g. 'plan 1, 3, 5')."* → hand off to [`plans.md`](plans.md).
