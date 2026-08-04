---
name: Run a DNAnexus app and monitor the resulting job
description: Find a published app, read its input specification, launch it idempotently with a nonce, poll the job to a terminal state, and read failureReason correctly when it fails.
api: https://api.dnanexus.com
docs: https://documentation.dnanexus.com/developer/api/running-analyses/apps
method: generated
generated: '2026-08-04'
grounded_in: https://documentation.dnanexus.com/developer/api/api-directory
operations:
  - /system/findApps
  - /app-xxxx[/yyyy]/describe
  - /app-xxxx[/yyyy]/run
  - /job-xxxx/describe
  - /job-xxxx/terminate
  - /system/findExecutions
---

# Run a DNAnexus app and monitor the job

## Steps

1. **Find the app.**
   `POST /system/findApps` with `{"published": true, "name": {"glob": "bwa*"}, "limit": 50}`. Paginate with `limit` + `starting`, following `next`.
   Apps resolve by ID (`app-9z80yBpyjv967GgZjkz00001`), by name (`app-bwa`), by name/version (`app-bwa/1.3`) or by name/tag (`app-bwa/unstable`).

2. **Read the input specification.**
   `POST /app-xxxx[/yyyy]/describe` with `{}` to get `inputSpec`, `outputSpec`, `runSpec` and the regions the app is available in. Build the input hash from `inputSpec` — do not guess field names.

3. **Launch it — with a nonce.**
   `POST /app-xxxx[/yyyy]/run` with
   `{"project": "project-…", "input": {…}, "folder": "/out", "name": "run-2026-08-04", "nonce": "<unique-string>"}`.
   The nonce makes the launch idempotent: a retry within one hour returns the original job rather than starting a second, billable run. Never retry a run without one.
   The response carries the new `job-xxxx` ID.

4. **Poll to a terminal state.**
   `POST /job-xxxx/describe` with `{}`. Watch `state` through the documented job lifecycle to a terminal state, backing off between polls — the account limit is 200 API calls/second.

5. **Read failures properly.**
   On failure, `describe` returns `failureReason` plus `failureMessage`. Route on `failureReason`:
   - `AppError` / `AppInternalError` — the executable itself failed; the message comes from the app. Billed.
   - `AppInsufficientResourceError` — out of memory or storage. With org policy `allowInstanceUpgradeOnJobRestart=true` and a retry in the execution policy, the platform retries one instance size up in the same family.
   - `InputError` / `OutputError` — fix the input or output hash; retrying unchanged will fail again.
   - `SpotInstanceInterruption` / `UnresponsiveWorker` — infrastructure; safe to relaunch.
   - `JMInternalError` — platform-side, and not billed.
   - `AuthError` — the launching token was revoked or expired. Job tokens live at most 30 days and a job tree inherits the root execution's expiry.
   - `CostLimitExceeded` / `SpendingLimitExceeded` / `OrgExpired` — billing, not code.

6. **Stop a runaway job.**
   `POST /job-xxxx/terminate` with `{}`. Terminating a root execution terminates its tree.

7. **Audit later.**
   `POST /system/findExecutions` to list jobs and analyses by project, state, executable or time window.

## Rules

- Distinguish the two error registries: HTTP-level failures use the `{"error": {"type", "message"}}` envelope; execution failures surface as `failureReason` on the job describe hash.
- Only `5xx` HTTP responses are retryable. Pair every retry of `/app-xxxx[/yyyy]/run` with the original nonce and an unchanged body.
