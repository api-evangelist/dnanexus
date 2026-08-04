---
name: Submit and review a Trusted Research Environment data access request
description: Discover a TRE, create a data access request (TRE application), attach cohort metadata and collaborators, submit it for review, and move it through the configured review steps to approval or rejection.
api: https://api.dnanexus.com
docs: https://documentation.dnanexus.com/developer/api/trusted-research-environments/data-access-requests
method: generated
generated: '2026-08-04'
grounded_in: https://documentation.dnanexus.com/developer/api/api-directory
operations:
  - /system/findTres
  - /tre-xxxx/describe
  - /tre-xxxx/getDataTypeGroups
  - /treApplication/new
  - /treApplication-xxxx/createCohortMetadata
  - /treApplication-xxxx/addCollaborators
  - /treApplication-xxxx/submit
  - /treApplication-xxxx/describe
  - /treApplication-xxxx/approve
  - /treApplication-xxxx/reject
  - /system/findTreApplications
---

# Submit and review a TRE data access request

A Trusted Research Environment (`tre-<handle>`, e.g. `tre-genomics`) is the governed boundary around a dataset. A **TRE application** (`treApplication-xxxx`) is the data access request a researcher submits against it.

## Requester path

1. **Find the TRE.**
   `POST /system/findTres` with `{}` (paginate with `limit`/`starting`/`next`), then `POST /tre-xxxx/describe` with `{}` to read its policies, inventory and review configuration.

2. **Read what can be requested.**
   `POST /tre-xxxx/getDataTypeGroups` with `{}` to enumerate the data type groups available in this TRE.

3. **Create the application.**
   `POST /treApplication/new` with the TRE, the requested data type groups and the research purpose.

4. **Attach cohort metadata.**
   `POST /treApplication-xxxx/createCohortMetadata` to describe the cohort being requested. Use `/treApplication-xxxx/updateCohortMetadata` to revise it and `/treApplication-xxxx/describeCohortMetadata` to read it back. Where a TRE has no Data Showcase, it enforces full-cohort selection and the request covers the complete participant population.

5. **Add collaborators.**
   `POST /treApplication-xxxx/addCollaborators` with the `user-<handle>` IDs who will work in the resulting environment. `/treApplication-xxxx/removeCollaborators` reverses it.

6. **Submit for review.**
   `POST /treApplication-xxxx/submit` with `{}`. Do not treat submission as approval — poll `POST /treApplication-xxxx/describe` for the review state.

## Reviewer / TRE admin path

1. **List pending requests.**
   `POST /system/findTreApplications` with the TRE and the state you are triaging, draining the `next` cursor.

2. **Read the request in full.**
   `POST /treApplication-xxxx/describe` with `{}`, plus `/treApplication-xxxx/describeCohortMetadata` for the requested cohort.

3. **Decide.**
   `POST /treApplication-xxxx/approve` or `POST /treApplication-xxxx/reject`. Both are consequential, non-reversible governance actions — require an explicit human decision; never let an agent approve autonomously.

4. **Configure review governance (TRE admins).**
   `/tre-xxxx/addApplicationReviewStep`, `/tre-xxxx/updateApplicationReviewStep`, `/tre-xxxx/removeApplicationReviewStep` shape the review chain; `/tre-xxxx/addApplicationReviewers` and `/tre-xxxx/removeApplicationReviewers` control who reviews; `/tre-xxxx/setPolicies` and `/tre-xxxx/setInventory` set the governed surface; `/tre-xxxx/activate` and `/tre-xxxx/deactivate` gate the whole TRE.

## Rules

- This surface governs access to human subject data. `approve`, `reject`, `setPolicies` and `deactivate` are human-in-the-loop operations by policy, whatever the token permits.
- `PermissionDenied` (401) on the review methods means the caller is not a reviewer or TRE admin — do not work around it.
- All activity here lands in the platform audit trail, which is retained to support 21 CFR Part 11 compliance.
