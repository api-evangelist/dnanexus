# DNAnexus

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **Not from the company, and here with a question?** You are welcome here — we would rather be the
> front line and point you the right way than have a good report go nowhere. What this repository
> can answer is narrow, though, so it is worth knowing who you are actually looking for:
>
> - **A question about how the API works, an account, billing, or a bug in the service** — that is
>   the company's own support, not us. We profile this API; we do not operate it and cannot see
>   your account.
> - **A bug in an open-source project we only catalog** — file it on that project's own repository.
>   This has happened with a real and correct bug report that reached us instead of the people who
>   could fix it, which helped nobody.
> - **Anything about this listing itself** — the description, the tags, the rating, a missing or
>   wrong artifact — is ours. Open an issue here.
> - **Not sure, or something general about API Evangelist or APIs.io** — open an issue on the
>   [APIs.io Inbox](https://github.com/api-search/inbox) and we will route it.
>
> **This repository contains no software, and we will never ask you to download anything.** There is
> no build, release, installer, or binary here — only text and machine-readable API descriptions, so
> there is nothing here that can be "corrupt" or need "repairing". Any issue, comment, or email
> claiming otherwise and offering a download link is not from us and is hostile. Do not follow the
> link; it is a lure. Report it to GitHub and, if you like, tell us at
> [info@apievangelist.com](mailto:info@apievangelist.com) so we can take it down.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

DNAnexus operates a cloud-based precision health data platform for genomic and multi-omic research, clinical testing and biopharma R&D. The DNAnexus Platform exposes a documented HTTP API of roughly 206 JSON-over-HTTPS methods at api.dnanexus.com covering projects and data containers, files and other data objects, apps, applets, workflows and analyses, jobs and execution containers, organizations and users, the Omics Data Catalog, and Trusted Research Environments (TREs) with data access requests. Every call is an HTTP POST with a JSON body, authenticated with a bearer token, versioned through an optional DNAnexus-API header, and made idempotent through a documented nonce field on object- and job-creation methods. First-party client libraries ship for Python (dxpy), Java, Scala and C++, alongside the dx command-line client, dxfuse and dxda, and the platform is an OpenID Connect provider for third-party sign-in and for job identity tokens.

**Areas:** Company · Genomics · Bioinformatics · Life Sciences · Healthcare · Cloud Computing · Data Platform · Scientific Computing · Precision Medicine · Clinical Research

## DNAnexus on APIs.io

Everything profiled for DNAnexus is published on APIs.io rather than in this repository. Start at the provider page — **[apis.io/providers/dnanexus/](https://apis.io/providers/dnanexus/)** — or go straight to one of the APIs or properties below.

### APIs (2)

- [DNAnexus Platform API](https://apis.io/apis/dnanexus/dnanexus-platform-api/) — The DNAnexus Platform API is a JSON-over-HTTPS API of roughly 206 documented methods. Each method is invoked with an HTTP POST to a route of the form…
- [DNAnexus OpenID Connect Provider](https://apis.io/apis/dnanexus/dnanexus-openid-connect-provider/) — DNAnexus operates an OpenID Connect provider that lets third-party web applications sign users in with their DNAnexus Platform credentials. The provider…

### Published properties

Each property below is a page on APIs.io carrying the machine-readable artifact and what it says.

#### Security (4)

Authentication, trust centre, and vulnerability disclosure.

- [Dnanexus Authentication](https://apis.io/security/dnanexus/dnanexus-authentication/)
- [Dnanexus Domain Security](https://apis.io/security/dnanexus/dnanexus-domain-security/)
- [Dnanexus Trust Center](https://apis.io/security/dnanexus/dnanexus-trust-center/)
- [Dnanexus Vulnerability Disclosure](https://apis.io/security/dnanexus/dnanexus-vulnerability-disclosure/)

#### OAuth scopes (1)

The permissions the API asks for.

- [Dnanexus Scopes](https://apis.io/scopes/dnanexus/dnanexus-scopes/)

#### Rate limits (1)

Published request limits.

- [Dnanexus Rate Limits](https://apis.io/rate-limits/dnanexus/dnanexus-rate-limits/)

## Where to find this API

Links to the company's own pages. This repository publishes no copy of their specifications — the canonical document is theirs and lives at the URLs below.

- [Website](https://www.dnanexus.com/)
- [Documentation](https://documentation.dnanexus.com/)
- [Getting started](https://documentation.dnanexus.com/getting-started)
- [API reference](https://documentation.dnanexus.com/developer/api/api-directory)
- [Pricing](https://academy.dnanexus.com/billing-access-and-orgs/billing_and_pricing)
- [Sign up](https://platform.dnanexus.com/register)
- [Support](https://community.dnanexus.com/s/)
- [Blog](https://blog.dnanexus.com/)
- [Security](https://trust.dnanexus.com)
- [Compliance](https://www.dnanexus.com/platform-security-compliance)
- [Terms of service](https://www.dnanexus.com/terms)
- [Privacy policy](https://www.dnanexus.com/privacy)
- [GitHub](https://github.com/dnanexus)
- [Status](https://status.dnanexus.com/)

## API Evangelist rating

**Kin Score 48.8** — developing · rubric 0.25.0 · scored 2026-10-23

| Facet | Score |
|---|---|
| Discoverability | 73.2 |
| Contract quality | 0.0 |
| Contract governance | 18.2 |
| Operational transparency | 65.8 |
| Developer ergonomics | 78.0 |
| Access clarity | 53.9 |

**Agent readiness 20.3** — agent-aware

| Agent-readiness dimension | Result |
|---|---|
| Spec Presence | — |
| Agentic Access | — |
| Reversibility Documented | — |
| MCP Server | — |
| Auth Clarity | served (partial credit) |
| Idempotency | documented (partial credit) |
| Error Semantics | documented (partial credit) |
| OpenAPI Examples | — |
| Rate Limit Signal | documented (partial credit) |
| Event Surface Described | — |
| Agent Skills | derived (partial credit) |
| Well Known Catalog | — |
| Consent Identity | — |
| Agent Card | — |
| Dry Run Mode | — |
| Delegated Identity | served (partial credit) |
| Protected Resource Metadata | — |
| Dynamic Client Registration | — |
| Agentic Commerce | — |
| Pagination Ratio | na |
| Operation Distinctiveness | na |

**Regulatory alignment 41.4** — Health

The rubric is published at [github.com/api-evangelist/kin-score](https://github.com/api-evangelist/kin-score). The per-check breakdown behind these numbers — which checks were earned, partial or missed — along with score history and how this provider compares to its peers, is available at [apis.io/providers/dnanexus/](https://apis.io/providers/dnanexus/).

## How this profile is made

API Evangelist profiles a company's public API surface the way a developer would find it: the website, developer portal and documentation, the specifications it publishes, its public repositories, and its pricing, status and changelog pages. From that it derives the properties above — contracts split per API, workflows, schemas, collections, governance rules, plans and rate limits — and scores them against the published [Kin Score rubric](https://github.com/api-evangelist/kin-score).

The artifacts are published on [APIs.io](https://apis.io), not in this repository. This repository is the public record for DNAnexus: the place to say something is wrong, missing or out of date, and to ask for a re-score or a removal.

## Open an issue

- **Something about this listing** — a wrong description, a missing API, a stale artifact or a question about the rating: [open an issue on this repository](https://github.com/api-evangelist/dnanexus/issues/new).
- **A new API, or one that is not in the catalog yet** — [submit it on APIs.io](https://apis.io/add/).
- **Anything else** — [the APIs.io inbox](https://github.com/api-search/inbox/issues/new) or [info@apievangelist.com](mailto:info@apievangelist.com).
