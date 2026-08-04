---
name: Authenticate against the DNAnexus Platform API and orient in a project
description: Establish a bearer-token session against api.dnanexus.com, confirm the identity behind the token, list the projects the token can reach, and read a project's contents before doing any work.
api: https://api.dnanexus.com
docs: https://documentation.dnanexus.com/developer/api
method: generated
generated: '2026-08-04'
grounded_in: https://documentation.dnanexus.com/developer/api/api-directory
operations:
  - /system/whoami
  - /system/findProjects
  - /project-xxxx/describe
  - /class-xxxx/listFolder
  - /system/resolveDataObjects
---

# Authenticate and orient on the DNAnexus Platform

## Before you start

- Base URL is `https://api.dnanexus.com`. **Every** method is an HTTP `POST` with a JSON body, even reads.
- Set `Content-Type: application/json` or omit it. Any other value returns `MalformedJSON` (400).
- Authenticate with `Authorization: Bearer <token>`. Tokens come from an interactive login, from an API token created in the platform UI (username dropdown → Profile → API Tokens), or, inside a job, from the execution environment.
- Optionally pin the API version with the `DNAnexus-API: 1.0.0` header. Omitting it selects the newest version.

## Steps

1. **Confirm who the token belongs to.**
   `POST /system/whoami` with body `{}`.
   A missing or bad token returns HTTP 401 `{"error":{"type":"InvalidAuthentication","message":"You need to be logged in to use this method"}}`. Stop and fix the token — do not retry.

2. **List reachable projects.**
   `POST /system/findProjects` with the permission level you need, e.g. `{"level": "CONTRIBUTE", "limit": 100}`.
   Permission levels are `VIEW`, `UPLOAD`, `CONTRIBUTE`, `ADMINISTER`. Paginate with `limit` + `starting`, following the `next` cursor in each response until it is `null`.

3. **Describe the chosen project.**
   `POST /project-xxxx/describe` (substitute the real `project-…` ID into the route) to read region, billTo, permission level and storage state before writing anything.

4. **Walk the folder tree.**
   `POST /class-xxxx/listFolder` with `{"folder": "/", "describe": true}` on the project ID to enumerate folders and objects.

5. **Resolve human paths to IDs.**
   `POST /system/resolveDataObjects` when you have a `project:/path/name` string rather than an object ID.

## Rules

- Never hard-code an entity ID: IDs are `class-xxxx` with 24 characters from `0123456789BFGJKPQVXYZbfgjkpqvxyz`. Users, orgs and TREs use handles instead (`user-joesmith`, `org-umbrellacorp`, `tre-genomics`).
- Stay under 200 API calls/second per account. `RateLimitConditional` (429) means back off.
- `5xx` responses are retryable; `4xx` responses are not — fix the request instead.
